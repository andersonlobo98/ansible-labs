# Lab 01 - Inventory Básico e Agrupamentos

Este laboratório tem como objetivo explorar a estrutura de um arquivo de inventário no Ansible, com aliases, tipos de conexão e agrupamentos.

---

## 📌 Questões teóricas

### 1. Qual o formato do inventário?

```ini
web ansible_host=webserver.com
db ansible_host=dbserver.com
```

**Resposta correta:** `ini` ✅

---

### 2. Qual porta o Ansible utiliza por padrão para conexão SSH?

**Resposta correta:** `22` ✅

---

### 3. Qual parâmetro é usado para indicar que a conexão é local (sem SSH)?

**Resposta correta:** `ansible_connection` ✅

---

### 4. Qual valor usar para `ansible_connection` ao se conectar a um servidor Windows?

**Resposta correta:** `winrm` ✅

---

## 🔧 Atividades práticas

### 1. Adicionar um novo host

Arquivo: `/home/bob/playbooks/inventory`

```bash
server1.company.com
server2.company.com
server3.company.com
```

Foi adicionado:

```bash
server4.company.com
```

---

### 2. Adicionar alias para `server4.company.com`

Antes:
```ini
web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
```

Adicionado:
```ini
db1 ansible_host=server4.company.com
```

---

### 3. Adicionar conexão Windows e credenciais

```ini
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
db1  ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123!
```

---

### 4. Adicionar grupos `web_servers` e `db_servers`

```ini
[web_servers]
web1
web2
web3

[db_servers]
db1
```

---

### 5. Adicionar grupo de grupos `all_servers`

```ini
[all_servers:children]
web_servers
db_servers
```

---

### 6. Estrutura complexa com múltiplos grupos e subgrupos

#### Tabela base:

| Alias       | Host             | OS    | Usuário        | Senha     |
|-------------|------------------|-------|----------------|-----------|
| sql_db1     | sql01.xyz.com    | Linux | root           | Lin$Pass  |
| sql_db2     | sql02.xyz.com    | Linux | root           | Lin$Pass  |
| web_node1   | web01.xyz.com    | Win   | administrator  | Win$Pass  |
| web_node2   | web02.xyz.com    | Win   | administrator  | Win$Pass  |
| web_node3   | web03.xyz.com    | Win   | administrator  | Win$Pass  |

#### Inventário final:

```ini
# Hosts
web_node1 ansible_host=web01.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node2 ansible_host=web02.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
web_node3 ansible_host=web03.xyz.com ansible_connection=winrm ansible_user=administrator ansible_password=Win$Pass
sql_db1   ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2   ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass

# Grupos
[db_nodes]
sql_db1
sql_db2

[web_nodes]
web_node1
web_node2
web_node3

[boston_nodes]
sql_db1
web_node1

[dallas_nodes]
sql_db2
web_node2
web_node3

[us_nodes:children]
boston_nodes
dallas_nodes
```

---

### ✅ Resultado

Todos os requisitos do laboratório foram concluídos com sucesso.