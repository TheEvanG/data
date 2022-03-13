import pandas as pd

url = 'https://raw.githubusercontent.com/TheEvanG/data/main/F-F_Research_Data_5.csv'

df1 = pd.read_csv(url)
df1.set_index('Date', inplace=True)
# proxy for growth and inflation
x = ['Mkt-RF', 'RF']
y = ['SMB', 'HML', 'RMW', 'CMA']
def regime_generate(df, rolling_window=None):
  #regime binary: compare with rolling mean
  if rolling_window:
    df = df - df.rolling(rolling_window).mean()
  else:
    df = df - df.mean()
  df = df.dropna()
  df = df.applymap(lambda x: 0 if x < 0 else 1)
  # using binary to demicala algo to convert a series of 0,1 to one integer
  df = df.apply(lambda x: pd.np.array([2**i for i in range(len(x))]).T.dot(x.array), axis=1)
  return df
df1['Regime'] = regime_generate(df1[x], 6)
#print(df1['Regime'].max())
df1 = df1.reset_index().set_index(['Regime', 'Date'], drop=True).sort_index()
print(df1)
print(df1.groupby('Regime').agg(['mean', 'std', 'skew']))
