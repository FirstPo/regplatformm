FROM python:3.12-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    xvfb xauth dbus-x11 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV DISPLAY=:99

# 启动时按需安装 Camoufox（避免远程构建阶段失败）
CMD ["sh", "-c", "python -c 'from camoufox.utils import install; install()' || true; Xvfb :99 -screen 0 1280x720x24 -nolisten tcp & sleep 1 && python api_solver.py --browser_type camoufox --no-headless --thread ${SOLVER_THREADS:-2} --host 0.0.0.0 --port ${SOLVER_PORT:-5072}"]

EXPOSE 5072
