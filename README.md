# Projeto DevOps com Vagrant e Ansible

## 🎓 Disciplina: Infraestrutura como Código – IFPB  
**Professor:** [Nome do Professor]  
**Alunos:**
- Nilson – 20221380002
- Wellington – 20221380031

---

## 📌 Objetivo
Automatizar a criação e configuração de uma infraestrutura virtual composta por 4 máquinas, utilizando Vagrant e Ansible para simular um ambiente DevOps com múltiplos serviços (Apache, MariaDB, NFS, DHCP, etc).

---

## ⚙️ Tecnologias Utilizadas

- Debian Bookworm (box `debian/bookworm64`)
- Vagrant + VirtualBox
- Ansible

---

## 🖥️ Máquinas Virtuais

| Hostname                     | IP                | Função              |
|-----------------------------|-------------------|---------------------|
| arq.nilson.wellington.devops| 192.168.56.31     | Arquivos, NFS, DHCP |
| db.nilson.wellington.devops | 192.168.56.17     | Banco de dados      |
| app.nilson.wellington.devops| 192.168.56.18     | Servidor Web        |
| cli.nilson.wellington.devops| 192.168.56.19     | Cliente Desktop     |

---

## 🧩 Serviços Configurados

### Todas as máquinas:
- Atualização do sistema e pacotes essenciais
- NTP (`chrony`) com pool.ntp.br
- Timezone: America/Recife
- Grupo `ifpb` + usuários `nilson` e `wellington`
- SSH com chave pública + aviso legal
- Sudo liberado sem senha para `vagrant` e `ifpb`
- Cliente NFS
- `autofs` nas máquinas `cli`, `app` e `db`

### arq:
- DHCP autoritativo com escopo `192.168.56.50-100`
- LVM: VG `dados`, LV `ifpb` com 15GB montado em `/dados`
- NFS exportando `/dados/nfs` para a rede
- Usuário `nfs-ifpb` com escrita exclusiva

### db:
- MariaDB
- Montagem automática via autofs em `/var/nfs`

### app:
- Apache2 com `index.html` personalizado
- Montagem automática via autofs

### cli:
- Firefox-ESR + X11 forwarding
- Montagem automática via autofs

---

## 🚀 Instruções de Execução

```bash
# Clonar o repositório
git clone https://github.com/seuusuario/projeto-devops.git
cd projeto-devops

# Subir infraestrutura
vagrant up

# Acessar máquina de controle (host local)
ansible -i ansible/hosts.ini all -m ping

# Executar os playbooks
ansible-playbook -i ansible/hosts.ini ansible/playbooks/base.yml
ansible-playbook -i ansible/hosts.ini ansible/playbooks/ssh.yml
# e assim por diante...
```

---

## 📎 Créditos
Este projeto foi desenvolvido como atividade prática da disciplina **Infraestrutura como Código (IaC)** do **IFPB**, com foco em práticas DevOps e automação.
