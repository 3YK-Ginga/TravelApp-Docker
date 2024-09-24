FROM python:3.12
WORKDIR /workspace
COPY ./source /workspace

# モジュールインストールコマンド
RUN pip install --no-cache-dir -r requirements.lock

# 【バックエンド】 サーバー起動自動化コマンド。ymlのcommandが有効だとこれは実行されない。
# mysql起動時間の問題から30秒空けてから実行している。
CMD ["/bin/sh", "-c", "sleep 30 && python manage.py runserver 0.0.0.0:8000"]

EXPOSE 8000