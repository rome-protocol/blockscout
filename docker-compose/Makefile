#!/bin/bash

ROLLUP_ENV := envs/rollup.env
FRONTEND_ENV := envs/common-frontend.env

.PHONY: ${ROLLUP_ENV}
${ROLLUP_ENV}:
	@if [ -z "${CHAIN_ID}" ]; then echo "CHAIN_ID is not set"; exit 1; fi
	@if [ -z "${ROLLUP_NAME}" ]; then echo "ROLLUP_NAME is not set"; exit 1; fi
	@echo "CHAIN_ID=${CHAIN_ID}" > "${ROLLUP_ENV}"
	@echo "SUBNETWORK=${ROLLUP_NAME}" >> "${ROLLUP_ENV}"
	@cat "${ROLLUP_ENV}"


.PHONY: ${FRONTEND_ENV}
${FRONTEND_ENV}:
	@if [ -z "${CHAIN_ID}" ]; then echo "CHAIN_ID is not set"; exit 1; fi
	@if [ -z "${ROLLUP_NAME}" ]; then echo "ROLLUP_NAME is not set"; exit 1; fi
	@if [ -z "${BLOCKSCOUT_PORT}" ]; then echo "BLOCKSCOUT_PORT is not set"; exit 1; fi
	@echo "NEXT_PUBLIC_NETWORK_ID=${CHAIN_ID}" > "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_NAME=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_SHORT_NAME=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_CURRENCY_NAME=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_CURRENCY_SYMBOL=${ROLLUP_NAME}" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_NETWORK_CURRENCY_DECIMALS=18" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_HOST=${ROLLUP_NAME}-client.devnet.romeprotocol.xyz" >> "${FRONTEND_ENV}"
	@echo "NEXT_PUBLIC_API_PORT=${BLOCKSCOUT_PORT}" >> "${FRONTEND_ENV}"
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
	@cat "${FRONTEND_ENV}"

config: ${ROLLUP_ENV} ${FRONTEND_ENV}
	@echo "Config complete"

start: docker compose -f geth.yml up
