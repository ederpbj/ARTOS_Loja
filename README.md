### Código

```bash
# Deploy rápido
git add admin.html index.html LEIA-ME.txt
git commit -m "Login de teste com acesso direto ao painel"
git remote -v   # confira se aponta para https://github.com/ederpbj/ARTOS_Loja.git
git push origin main

# primeira vez no servidor
ssh root@147.79.110.195
mkdir -p /var/www/projetos && cd /var/www/projetos

# gerar token git key
ssh-keygen -t ed25519 -C "deploy-artos" -f ~/.ssh/artos_deploy -N ""
cat ~/.ssh/artos_deploy.pub

cat >> ~/.ssh/config <<'EOF'
Host github-artos
  HostName github.com
  User git
  IdentityFile ~/.ssh/artos_deploy
  IdentitiesOnly yes
EOF

cd /var/www/projetos
git clone git@github-artos:ederpbj/ARTOS_Loja.git artos

# se perfil público, pode usar:
git clone https://github.com/ederpbj/ARTOS_Loja.git artos

# atualizações seguintes
ssh root@147.79.110.195 'cd /var/www/projetos/artos && git pull origin main'

# Servir o site
server {
    listen 80;
    server_name seudominio.com.br;   # ou o IP
    root /var/www/projetos/artos;
    index index.html;
}


# restart do nginx
nginx -t && systemctl reload nginx
```
