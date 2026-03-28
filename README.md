# Data-Analysis-of-Hotel-Booking

import pandas as pd
import matplotlib.pyplot as plt
# import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
a = pd.read_csv('hotel_bookings_MLE_Projject.csv')

a.head()
a.tail()
a.tail(15)

a.shape()
a.columns()
a.info()
a.describe()
a.isnull().sum()
a.duplicated().sum()
a.nunique()
a.corr()

a['reservation_status_date'] = pd.to_datetime(a['reservation_status_date'], dayfirst=True)

# a.info()

for col in a.describe(include = 'object').columns:
  print(col)
  print(a[col].unique())

a.describe(include = 'object')


a.isnull().sum()


b = sum(a.isnull().sum())
b

a.describe()

a['adr'].plot(kind = 'box')
# a = a[a['adr'] < 5000]
# a

a['adr'].plot(kind = 'box')

cancelled_perc = a['is_canceled'].value_counts (normalize = True)
print(cancelled_perc)
plt.figure(figsize = (5,4))
plt.title('Reservation status count')
plt.bar(['Not canceled', 'Canceled'], a['is_canceled'].value_counts(), edgecolor = 'k', width = 0.5)
plt.show()

# # import matplotlib.pyplot as plt
# plt.figure(figsize = (8,4))
# # ax1 = plt.countplot(x = 'hotel', hue = 'is_canceled', data = a, palette = 'Blues')
# # legend_labels,_ = axl. get_legend_handles_labels()
# ax1.legend(bbox_to_anchor = (1,1))
# plt.title('Reservation status in different hotels', size = 20)
# plt.xlabel( 'hotel')
# plt.ylabel('number of reservations')
# plt.legend(['not canceled', 'canceled'])
# plt.show()

import matplotlib.pyplot as plt

# Count reservations by hotel and cancellation status
counts = a.groupby(['hotel', 'is_canceled']).size().unstack()

# Plot
counts.plot(kind='bar', figsize=(7, 4))

plt.title('Reservation Status in Different Hotels', fontsize=20)
plt.xlabel('Hotel')
plt.ylabel('Number of Reservations')
plt.legend(['Not Canceled', 'Canceled'], bbox_to_anchor=(1, 1))

plt.tight_layout()
plt.show()

resort_hotel = a[a['hotel'] == 'Resort Hotel'] # Fixed hotel name 'Resort_Hotel' to 'Resort Hotel'
resort_hotel['is_canceled'].value_counts(normalize = True)

city_hotel = a[a['hotel'] == 'City Hotel'] # Fixed hotel name 'Resort_Hotel' to 'City Hotel'
city_hotel['is_canceled'].value_counts(normalize = True)

resort_hotel = resort_hotel.groupby('reservation_status_date')[['adr']].mean()
city_hotel = city_hotel.groupby('reservation_status_date')[['adr']].mean()
# plt.figure(figsize = (20,8))
# plt.title('Average Daily Rate in City and Resort Hotel', fontsize = 38)
# plt.plot(resort_hotel.index, resort_hotel['adr'], label='Resort Hotel')

plt.figure(figsize = (20,8))
plt.title('Average Daily Rate in City and Resort Hotel', fontsize = 38)
plt.plot(resort_hotel.index, resort_hotel['adr'], label='Resort Hotel')
plt.plot(city_hotel.index, city_hotel['adr'], label='City_Hotel')
plt.legend(fontsize = 20)
plt.show()

import matplotlib.pyplot as plt

# Extract month
a['month'] = a['reservation_status_date'].dt.month

# Count reservations by month and cancellation status
month_counts = a.groupby(['month', 'is_canceled']).size().unstack()

# Plot
month_counts.plot(kind='bar', figsize=(16, 8))

plt.title('Reservation Status per Month', fontsize=20)
plt.xlabel('Month')
plt.ylabel('Number of Reservations')
plt.legend(['Not Canceled', 'Canceled'], bbox_to_anchor=(1, 2))

plt.tight_layout()
plt.show()

import matplotlib.pyplot as plt
import seaborn as sns # Added seaborn import
plt.figure(figsize = (15,8))
plt.title('ADR per month', fontsize=30)
sns.barplot(x='month', y='adr', data = a[a['is_canceled'] == 1].groupby('month')[['adr']].sum().reset_index()) # Changed sns.barplot usage to specify x and y
plt.show()

