# byod-validator

# Script: byod_validator.py

1. Setup
   - Load IMEIs from imei.txt (one per line)
   - Launch browser (stealth mode, rotating fingerprint)
   - Navigate to https://www.att.com/buy/byod/identify?devicetype=phone

2. Wait for page ready
   - wait_for_selector("#IMEI-validator-input-field", timeout=15s)
   - If timeout → abort

3. Loop each IMEI:
   a. Focus input field
      - page.click("#IMEI-validator-input-field")
      
   b. Type IMEI
      - page.fill("#IMEI-validator-input-field", imei)
      - page.press("#IMEI-validator-input-field", "Enter")
      
   c. Wait for result (10s timeout):
      - Try selector: div[data-testid="byod-sim"]
      - Try selector: svg[aria-label="Alert Icon"]
      
   d. Parse result:
      - If byod-sim found → "COMPATIBLE"
      - If Alert Icon found → "INCOMPATIBLE"
      - If neither → abort("Unexpected state")
      
   e. Log output:
      - {imei, status, timestamp}
      
   f. Reset form:
      - page.reload() or clear input
      - Wait 3 seconds (rate limit safety)

4. Output
   - Save results to run1_output.json
   - Close browser

5. Repeat (Run 2)
   - Same process → run2_output.json
```
