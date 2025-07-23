# Projeto DevOps com Vagrant e Ansible

## 🎓 Disciplina: Administração de Sistemas Abertos  
**Período:** 2025.1  
**Professor:** Leonidas Lima  
**Instituição:** IFPB - Campus João Pessoa

## 👥 Equipe
- **Nilson Vinícius Aurelio Chaves** - Matrícula: 20221380002  
- **Wellington Antonio da Silva** - Matrícula: 20221380031

---

## 🎯 Objetivo

Automatizar o provisionamento de uma infraestrutura virtual com 4 máquinas Linux (Debian Bookworm), utilizando **Vagrant** e **Ansible**, para demonstrar práticas de **Infraestrutura como Código (IaC)** e **DevOps**.

---

## 🖥️ Estrutura da Infraestrutura

| Máquina | Função                | Hostname                         | IP                     | Observações                   |
|--------|------------------------|----------------------------------|------------------------|-------------------------------|
| `arq`  | Servidor de Arquivos   | `arq.nilson.wellington.devops`  | 192.168.56.102 (fixo)   | DHCP, NFS, LVM                |
| `db`   | Banco de Dados         | `db.nilson.wellington.devops`   | DHCP                    | MariaDB, autofs               |
| `app`  | Servidor Web           | `app.nilson.wellington.devops`  | DHCP                    | Apache2, autofs               |
| `cli`  | Estação Cliente        | `cli.nilson.wellington.devops`  | DHCP                    | Firefox, X11, autofs          |

---

## ⚙️ Ferramentas Utilizadas

- [Vagrant](https://www.vagrantup.com/) (com VirtualBox)
- [Ansible](https://www.ansible.com/)
- Linux Debian (bookworm64)

---

## ⚙️ Automatizações com Ansible

### Comum a todas as VMs:
- Atualização do sistema
- Timezone: `America/Recife`
- NTP configurado com `chrony` (pool.ntp.br)
- Grupo `ifpb` e usuários `nilson` e `wellington`
- SSH:
  - Autenticação por chave pública
  - Root bloqueado
  - Acesso só para `vagrant` e `ifpb`
  - Mensagem de acesso
- Cliente NFS
- Sudo sem senha para o grupo `ifpb`

### Servidor `arq`:
- DHCP autoritativo
- LVM com 3 discos de 10 GB → VG `dados`, LV `ifpb` (15 GB)
- Montagem automática em `/dados`
- Servidor NFS exportando `/dados/nfs`
- Usuário exclusivo `nfs-ifpb` com UID/GID 65534

### Servidor `db`:
- Instalação do `mariadb-server`
- Autofs montando `/dados/nfs` do `arq` em `/var/nfs`

### Servidor `app`:
- Instalação do Apache2
- Substituição do `index.html` com dados do projeto
- Autofs montando `/dados/nfs` em `/var/nfs`

### Cliente `cli`:
- Instalação de `firefox-esr` e `xauth`
- SSH com `X11Forwarding` habilitado
- Montagem automática do NFS em `/var/nfs`

---

## 📁 Estrutura de Diretórios

```bash
projeto-devops/
├── ansible/
│   ├── comum-playbook.yml
│   ├── arq-playbook.yml
│   ├── db-playbook.yml
│   ├── app-playbook.yml
│   ├── cli-playbook.yml
│   ├── inventory.ini
│   └── keys/
├── .vagrant/           # Discos dinâmicos criados para a VM 'arq'
├── Vagrantfile         # Define as VMs e provisionamento
└── README.md
```

---

## 🚀 Como Executar o Projeto

### 1. Clone o repositório
```bash
git clone https://github.com/Nilson-Chaves/projeto-devops.git
cd projeto-devops
```

### 2. Suba as máquinas com o Vagrant junto com os playbooks de todas as máquinas (tudo integrado no mesmo comando)
```bash
vagrant up
```

---

## ✅ Critérios Atendidos

✔️ Vagrant com todas as especificações  
✔️ Execução dos playbooks sem erros  
✔️ Organização em pastas por máquina  
✔️ Documentação clara, organizada e completa

---

## 📦 Entrega

Este projeto é composto por:
- `Vagrantfile`
- `playbooks` organizados por máquina
- `README.md`
