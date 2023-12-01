# Flight-prediction
# Using python in colab
# Flight price prediction   Make a csv file of a flight's prices with the following attributes - flight_no , flight_name, source , destination, date, economy_classprice, business_classprice, time, day. This database contains flight informations of 100 flights from Kolkata to Delhi. The plots required for - 1) date vs price 2) time vs price 3) flight_name vs price 4) optional ones if we can add.



# Creating the csv automated file
import pandas as pd 
import random 
from datetime import datetime, timedelta

flight_data = []
previous_date = datetime(2023, 1, 1)
days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
j = 0

for i in range(100):
    flight_no = f"C12{i+1:03d}"
    flight_name = random.choice(["Air India", "SpiceJet", "Indigo", "SpiceJet Premium", 
                                 "AirAsia Premium", "Vistara Premium", "Go Air", "Go First", "Jet Airways", "Air Asia", "Vistara"])
    source = "Kolkata"
    destination = "Delhi"
    date = previous_date + timedelta(days=1)
    previous_date = date
    economy_classprice = random.randint(5500, 9000)
    business_classprice = random.randint(10000, 15000)
    time = random.choice(["1:15", "2:50", "3:45", "6:15", "8:10", "09:30", "12:30", "14:30", "16:00", "18:45", "21:15", "23:30"])
    day = days[j]
    if j == 6:
        j = 0
    else:
        j += 1
    flight_data.append([flight_no, flight_name, source, destination, date, economy_classprice, business_classprice, time, day])

columns = ['flight_no', 'flight_name', 'source', 'destination', 'date', 'economy_classprice', 'business_classprice', 'time', 'day']
df = pd.DataFrame(flight_data, columns=columns)

# for printing the data base
print(df.to_string(header=True, index=False))

# importing LIBRARY 
import pandas as pd
import matplotlib.pyplot as plt
df.to_csv('flight_prices.csv', index=False)

# Date vs Price
df.sort_values('date', inplace=True)
plt.figure(figsize=(25, 8))
plt.plot(df['date'], df['economy_classprice'], label='Economy Class', color='blue')
plt.plot(df['date'], df['business_classprice'], label='Business Class', color='red')
plt.title('Date vs Class Price')
plt.xlabel('Date')
plt.ylabel('Class Price')
plt.xticks(rotation=90)
plt.grid(True)
plt.legend()
plt.show()

# Time vs Price
plt.figure(figsize=(25, 8))
plt.scatter(df['time'], df['economy_classprice'], label='Economy Class', color='blue')
plt.scatter(df['time'], df['business_classprice'], label='Business Class', color='red')
plt.title('Time vs Flight Price')
plt.xlabel('Time')
plt.ylabel('Flight Price')
plt.grid(True)
plt.show()

# flight name vs flight classes price
grouped = df.groupby('flight_name')[['economy_classprice', 'business_classprice']].mean().reset_index()
plt.figure(figsize=(20, 8))
bar_width = 0.35
index = grouped.index
plt.bar(index, grouped['economy_classprice'], bar_width, label='Economy Class Price')
plt.bar(index + bar_width, grouped['business_classprice'], bar_width, label='Business Class Price',color='red')
plt.xlabel('Flight Name')
plt.ylabel('Flight Price')
plt.title('Economy vs Business Class Prices for Flights')
plt.xticks(index + bar_width / 2, grouped['flight_name'], rotation=0)  # Rotating labels for better readability
plt.legend()
plt.tight_layout()
plt.show()

# Day vs Price Distribution
plt.figure(figsize=(20, 8))
plt.scatter(df['day'], df['economy_classprice'], label='Economy Class', color='blue', alpha=0.5)
plt.scatter(df['day'], df['business_classprice'], label='Business Class', color='red', alpha=0.5)
plt.title('Day vs Price Distribution')
plt.xlabel('Day')
plt.ylabel('Price')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()
