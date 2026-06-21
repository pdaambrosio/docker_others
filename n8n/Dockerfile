ARG N8N_VERSION=1.91.3
FROM n8nio/n8n:${N8N_VERSION}

LABEL org.opencontainers.image.title="n8n Workflow Automation" \
      org.opencontainers.image.description="Self-hosted n8n with production hardening" \
      org.opencontainers.image.version="${N8N_VERSION}" \
      org.opencontainers.image.source="https://github.com/n8n-io/n8n"

USER root
RUN apk add --no-cache \
      tzdata \
      curl \
    && rm -rf /var/cache/apk/*

RUN mkdir -p /home/node/.n8n/custom && \
    chown -R node:node /home/node/.n8n && \
    chmod 700 /home/node/.n8n

USER node
WORKDIR /home/node
ENV N8N_PORT=5678 \
    N8N_HOST=0.0.0.0 \
    N8N_PROTOCOL=http \
    GENERIC_TIMEZONE=UTC \
    TZ=UTC \
    NODE_OPTIONS="--max-old-space-size=512" \
    N8N_DIAGNOSTICS_ENABLED=false \
    N8N_VERSION_NOTIFICATIONS_ENABLED=false \
    N8N_GRACEFUL_SHUTDOWN_TIMEOUT=30 \
    N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true

EXPOSE 5678

HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD curl -fs http://localhost:${N8N_PORT}/healthz || exit 1
