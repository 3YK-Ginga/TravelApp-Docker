FROM node:22
WORKDIR /workspace
COPY ./source /workspace

# パッケージインストールコマンド。npmとyarnは共存できません！
RUN npm install --no-cache

# 【フロントエンド】 サーバー起動自動化コマンド。ymlのcommandが有効だとこれは実行されない
CMD ["npm", "start"]

EXPOSE 3000