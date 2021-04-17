# Commands for ease of use
setup: sam-build step-setup ddb-create step-create sam-lambda-start
run: step-run ddb-count step-run-details
run-details: step-run-details
logs: step-logs
teardown: step-cleanup ddb-cleanup
results: step-run-details

# Granular commands
sam-build:
	sam build --use-container
sam-lambda-start:
	sam local start-lambda
ddb-create:
	aws dynamodb create-table \
	--table-name TransactionTable \
	--attribute-definitions AttributeName=Id,AttributeType=S \
	--key-schema AttributeName=Id,KeyType=HASH \
	--provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
	--endpoint-url http://localhost:8000
ddb-cleanup:
	rm -rf docker/dynamodb
ddb-count:
	aws dynamodb scan \
	--table-name TransactionTable \
	--select "COUNT" \
	--endpoint-url http://localhost:8000
ddb-delete:
	aws dynamodb delete-table \
	--table-name TransactionTable \
	--endpoint-url http://localhost:8000 
ddb-reset: ddb-delete ddb-create ddb-count
step-setup: step-cleanup
	mkdir -p docker/dynamodb
	docker compose up -d 
	while ! AWS_PAGER="" aws stepfunctions list-state-machines --endpoint http://localhost:8083; do sleep 0.1; done;
step-logs:
	docker compose logs --follow
step-cleanup:
	docker compose down
step-generate: # runtime replacement of 'DefinitionSubstitutions' parameters in state machine for use against local resources
	cat ./statemachine/stock_trader.asl.json \
	| jq -r '.States."Check Stock Value".Resource = "arn:aws:lambda:us-east-1:123456789012:function:StockCheckerFunction"' \
	| jq -r '.States."Sell Stock".Resource = "arn:aws:lambda:us-east-1:123456789012:function:StockSellerFunction"' \
	| jq -r '.States."Buy Stock".Resource = "arn:aws:lambda:us-east-1:123456789012:function:StockBuyerFunction"' \
	| jq -r '.States."Record Transaction".Resource = "arn:aws:states:::dynamodb:putItem"' \
	| jq -r '.States."Record Transaction".Parameters.TableName = "TransactionTable"' \
	> ./statemachine/stock_trader_local.asl.json
step-create: step-generate
	AWS_PAGER="" aws stepfunctions create-state-machine \
	--definition file://./statemachine/stock_trader_local.asl.json \
	--name "StockTrader" --role-arn "arn:aws:iam::012345678901:role/DummyRole" \
	--endpoint http://localhost:8083
step-update: step-generate
	AWS_PAGER="" aws stepfunctions update-state-machine \
	--definition file://./statemachine/stock_trader_local.asl.json \
	--state-machine arn:aws:states:us-east-1:123456789012:stateMachine:StockTrader \
	--endpoint http://localhost:8083
step-run:
	AWS_PAGER="" aws stepfunctions start-execution \
	--state-machine arn:aws:states:us-east-1:123456789012:stateMachine:StockTrader \
	--name $$(date '+%Y-%m-%d-%H-%M-%S') \
	--output yaml \
	--endpoint http://localhost:8083 --output json > ./statemachine/execution_local.json
step-list-runs:
	AWS_PAGER="" aws stepfunctions list-executions \
	--state-machine arn:aws:states:us-east-1:123456789012:stateMachine:StockTrader \
	--output yaml \
	--endpoint http://localhost:8083
step-run-details:
	while ! [ "RUNNING" != $$(AWS_PAGER="" aws stepfunctions \
	describe-execution --execution-arn $$(cat statemachine/execution_local.json | jq -r '.executionArn') \
	--output text \
	--query 'status' \
	--endpoint http://localhost:8083) ]; \
	do \
		sleep 0.1; \
	done;

	AWS_PAGER="" aws stepfunctions \
	describe-execution --execution-arn $$(cat statemachine/execution_local.json | jq -r '.executionArn') \
	--output yaml \
	--endpoint http://localhost:8083
