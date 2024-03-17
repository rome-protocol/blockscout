#!/bin/bash

ROLLUP_ENV := envs/rollup.env
FRONTEND_ENV := envs/common-frontend.env

.PHONY: ${ROLLUP_ENV}
${ROLLUP_ENV}:
	@echo "CHAIN_ID=${CHAIN_ID}" > "${ROLLUP_ENV}"
	@echo "SUBNETWORK=${ROLLUP_NAME}" >> "${ROLLUP_ENV}"
	@cat "${ROLLUP_ENV}"


.PHONY: ${FRONTEND_ENV}
${FRONTEND_ENV}:
	@echo "NEXT_PUBLIC_NETWORK_ID=${CHAIN_ID}" > "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_NAME=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_SHORT_NAME=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_CURRENCY_NAME=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_CURRENCY_SYMBOL=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_CURRENCY_DECIMALS=18" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_HOST=${ROLLUP_NAME}-client.devnet.romeprotocol.xyz" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_PORT=${PORT}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_PROTOCOL=http" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_STATS_API_HOST=http://${ROLLUP_NAME}-client.devnet.romeprotocol.xyz:8080" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_BASE_PATH=/" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_APP_HOST=${ROLLUP_NAME}-client.devnet.romeprotocol.xyz" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_APP_PROTOCOL=http" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_HOMEPAGE_CHARTS=['daily_txs']" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_VISUALIZE_API_HOST=http://${ROLLUP_NAME}-client.devnet.romeprotocol.xyz:8081" >> "${FRONTEND_ENV}"
	@echo "# NEXT_PUBLIC_IS_TESTNET=true" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_WEBSOCKET_PROTOCOL=ws" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_SPEC_URL=http://raw.githubusercontent.com/blockscout/blockscout-api-v2-swagger/main/swagger.yaml" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_AD_TEXT_PROVIDER=none" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_AD_BANNER_PROVIDER=none" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_LOGO=http://rome-public-assets.s3.us-east-1.amazonaws.com/rome-banner.jpeg" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_ICON=http://rome-public-assets.s3.us-east-1.amazonaws.com/rome-logo.png" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_FOOTER_LINKS=http://rome-public-assets.s3.us-east-1.amazonaws.com/footer.json" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_HOMEPAGE_PLATE_TEXT_COLOR=white" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_HOMEPAGE_PLATE_BACKGROUND=#a85cfa" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_HIDE_INDEXING_ALERT_BLOCKS=true" >> "${FRONTEND_ENV}"
	@echo "PERMANENT_LIGHT_MODE_ENABLED=true" >> "${FRONTEND_ENV}"
	@echo "Config written to ${FRONTEND_ENV}"

generate-nginx-yml:
	@echo "version: '3.9'" > services/nginx.yml
	@echo "" >> services/nginx.yml
	@echo "services:" >> services/nginx.yml
	@echo "  proxy:" >> services/nginx.yml
	@echo "    image: nginx" >> services/nginx.yml
	@echo "    container_name: proxy" >> services/nginx.yml
	@echo "    extra_hosts:" >> services/nginx.yml
	@echo "      - '172.17.0.1:host-gateway'" >> services/nginx.yml
	@echo "      - '${ROLLUP_NAME}-client.devnet.romeprotocol.xyz:host-gateway'" >> services/nginx.yml
	@echo "    volumes:" >> services/nginx.yml
	@echo "      - \"../proxy:/etc/nginx/templates\"" >> services/nginx.yml
	@echo "    environment:" >> services/nginx.yml
	@echo "      BACK_PROXY_PASS: http://backend:4000" >> services/nginx.yml
	@echo "      FRONT_PROXY_PASS: http://frontend:3000" >> services/nginx.yml
	@echo "    ports:" >> services/nginx.yml
	@echo "      - target: 80" >> services/nginx.yml
	@echo "        published: ${PORT}" >> services/nginx.yml
	@echo "      - target: 8080" >> services/nginx.yml
	@echo "        published: 8080" >> services/nginx.yml
	@echo "      - target: 8081" >> services/nginx.yml
	@echo "        published: 8081" >> services/nginx.yml

config: ${ROLLUP_ENV} ${FRONTEND_ENV}
	@if [ -z "${CHAIN_ID}" ]; then echo "CHAIN_ID is not set"; exit 1; fi
	@if [ -z "${ROLLUP_NAME}" ]; then echo "ROLLUP_NAME is not set"; exit 1; fi
	@if [ -z "${PORT}" ]; then echo "PORT is not set"; exit 1; fi
	@make generate-nginx-yml
	@echo "Config complete"

start:
	docker compose -f geth.yml up
