# Assignment Week 1: Bitcoin Transaction Visualizer

**Objective:** Create a simple Streamlit-based dashboard that allows users to visualize basic information about Bitcoin transactions, such as inputs, outputs, and transaction fees.

## Instructions:

1. **Please read the instructions!**

2. **Install the required libraries:** Install Python libraries, such as requests, pandas, and streamlit.

3. **Familiarize yourself with the Blockchair API documentation:** Review the API documentation at [https://blockchair.com/api/docs](https://blockchair.com/api/docs) to understand the available endpoints, request parameters, and response formats.

4. **Create a new Python file (e.g., app.py) and import the necessary libraries.**

```python
import streamlit as st
import requests
import pandas as pd
```

5. **Set up the Blockchair API key and base URL:**

```python
blockchair_base_url = "https://api.blockchair.com/bitcoin"
```

6. **Create a function to fetch transaction data using the Blockchair API:**

```python
def fetch_transaction_data(tx_hash):
    url = f"{blockchair_base_url}/dashboards/transaction/{tx_hash}"
    response = requests.get(url)

    if response.status_code == 200:
        data = response.json()
        return data
    else:
        return None
```

7. **Set up the Streamlit user interface:**
```python
st.title("Bitcoin Transaction Visualizer")

tx_hash_input = st.text_input("Enter a Bitcoin transaction hash:")
```

8. **Fetch and display transaction data:**

```python
if tx_hash_input:
    tx_data = fetch_transaction_data(tx_hash_input)

    if tx_data:
        st.subheader("Transaction Details")
        st.write(f"Transaction hash: {tx_data['data'][tx_hash_input]['transaction']['hash']}")
        st.write(f"Size: {tx_data['data'][tx_hash_input]['transaction']['size']} bytes")
        st.write(f"Fee: {tx_data['data'][tx_hash_input]['transaction']['fee']} satoshis")

        st.subheader("Inputs")
        inputs_df = pd.DataFrame(tx_data['data'][tx_hash_input]['inputs'])
        if not inputs_df.empty:
            st.dataframe(inputs_df[['recipient', 'value']])
        else:
            st.write("No inputs data available")

        st.subheader("Outputs")
        outputs_df = pd.DataFrame(tx_data['data'][tx_hash_input]['outputs'])
        if not outputs_df.empty:
            st.dataframe(outputs_df[['recipient', 'value']])
        else:
            st.write("No outputs data available")
    else:
        st.error("Error fetching transaction data. Please check the transaction hash and try again.")
```

9. **Run the Streamlit application:**

```shell
streamlit run app.py
```

