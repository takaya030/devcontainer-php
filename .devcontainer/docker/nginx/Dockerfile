FROM nginx:latest

# 1. nginxユーザー（101）のUID/GIDを 1000 に変更する
# 2. 関連するディレクトリの所有権を再帰的に更新する
RUN usermod -u 1000 nginx \
    && groupmod -g 1000 nginx \
    && chown -R nginx:nginx /var/cache/nginx /var/log/nginx /etc/nginx
