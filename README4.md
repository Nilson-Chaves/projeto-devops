# Projeto DevOps com Vagrant e Ansible

## 🎓 Disciplina
Administração de Sistemas Abertos  
**Professor**: Leonidas Lima

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
| `arq`  | Servidor de Arquivos   | `arq.nilson.wellington.devops`  | 192.168.56.31 (fixo)   | DHCP, NFS, LVM                |
| `db`   | Banco de Dados         | `db.nilson.wellington.devops`   | DHCP                   | MariaDB, autofs               |
| `app`  | Servidor Web           | `app.nilson.wellington.devops`  | DHCP                   | Apache2, autofs               |
| `cli`  | Estação Cliente        | `cli.nilson.wellington.devops`  | DHCP                   | Firefox, X11, autofs          |

---

## ⚙️ Ferramentas Utilizadas

- [Vagrant](https://www.vagrantup.com/) (com VirtualBox)
- [Ansible](https://www.ansible.com/)
- Linux Debian (bookworm64)

---

## 📁 Estrutura do Projeto

```
projeto-devops/
├── Vagrantfile
├── ansible/
│   ├── hosts.ini
│   ├── site.yml
│   ├── playbooks/
│   │   ├── common/
│   │   │   ├── base.yml
│   │   │   ├── ntp.yml
│   │   │   └── ssh.yml
│   │   ├── arq/
│   │   │   ├── lvm.yml
│   │   │   └── nfs-server.yml
│   │   ├── db/
│   │   │   ├── mariadb.yml
│   │   │   └── autofs.yml
│   │   ├── app/
│   │   │   ├── apache.yml
│   │   │   ├── autofs.yml
│   │   │   └── files/
│   │   │       └── index.html
│   │   ├── cli/
│   │   │   ├── firefox.yml
│   │   │   └── autofs.yml
│   │   └── nfs.yml
├── estrutura.txt
├── comandos.txt
└── README.md
```

---

## 🚀 Como Executar o Projeto

### 1. Clone o repositório
```bash
git clone https://github.com/Nilson-Chaves/projeto-devops.git
cd projeto-devops
```

### 2. Suba as máquinas com o Vagrant
```bash
vagrant up
```

### 3. Acesse a pasta Ansible e execute o playbook principal
```bash
cd ansible
ansible-playbook -i hosts.ini playbooks/site.yml
```

> Certifique-se que sua chave SSH pública está corretamente configurada, conforme o `ssh.yml`.

---

## ✅ Serviços e Configurações Automatizadas

- Atualização e pacotes essenciais
- Timezone America/Recife
- NTP (`chrony`)
- Criação de grupo `ifpb` e usuários `nilson` e `wellington`
- SSH com autenticação por chave e banner de aviso
- LVM e montagem automática de `/dados`
- NFS entre `arq` e demais máquinas
- MariaDB configurado no `db`
- Apache2 com `index.html` customizado no `app`
- Firefox e X11 exportado no `cli`

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
- `site.yml` integrador
- `README.md`
- `estrutura.txt` com a árvore do projeto
- `comandos.txt` com todos os comandos utilizados
