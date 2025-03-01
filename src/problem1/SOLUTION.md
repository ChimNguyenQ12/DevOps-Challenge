Provide your CLI command here:
grep '"symbol": "TSLA"' transaction-log.txt | sed -E 's/.*"order_id": "([^"]+)".*/\1/' | xargs -I {} curl -s "https://example.com/api/{}" >> output.txt

