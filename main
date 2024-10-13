import streamlit as st
import yfinance as yf
import matplotlib.pyplot as plt
import pandas as pd

# Set up the Streamlit page
st.title('Commodities Prices Dashboard')

# List of commodities you want to track (you can add more)
commodities = {
    'Gold': 'GC=F',
    'Silver': 'SI=F',
    'Crude Oil': 'CL=F',
    'Natural Gas': 'NG=F',
    'Corn': 'ZC=F',
    'Soybeans': 'ZS=F',
    'Wheat': 'ZW=F'
}
periods=['1d', '5d', '1mo', '3mo', '6mo', '1y', '5y', '10y', 'ytd', 'max']

# Allow user to select which commodities to display
selected_commodities = st.multiselect('Select commodities to display', list(commodities.keys()), default=list(commodities.keys()))
selected_period=st.selectbox("Select period",periods,index=2)
# Function to fetch data from yfinance
def fetch_data(ticker):
    data = yf.download(ticker, period, interval='1d')
    return data

# Fetch data for the selected commodities
commodity_data = {}
for commodity in selected_commodities:
    ticker = commodities[commodity]
    data = fetch_data(ticker,selected_period)
    commodity_data[commodity] = data

# Plot the data for each selected commodity
for commodity, data in commodity_data.items():
    st.subheader(f'{commodity} Prices (Last 1 Month)')
    st.line_chart(data['Close'])

# Option to download the data
if st.button('Download Data as CSV'):
    combined_data = pd.concat({k: v['Close'] for k, v in commodity_data.items()}, axis=1)
    combined_data.columns = selected_commodities
    csv = combined_data.to_csv().encode('utf-8')
    st.download_button(label='Download CSV', data=csv, file_name='commodity_prices.csv', mime='text/csv')

# Footer
st.write('Data Source: Yahoo Finance')
