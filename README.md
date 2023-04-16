# Project-3
#Zach Hendricks

import pandas as pd 

import matplotlib.pyplot as plt

data = pd.read_csv('heart.csv')

def getHeartAttack():
    #Get the heart attack probability
    Heart_Attack_mean = (data.attack.mean())
    #Take the probability and subtract 100
    No_HA_Probability = (1 - Heart_Attack_mean)
    #Print Statements
    print("The percent of no heart attacks in the data: %0.2f%%" % (No_HA_Probability * 100))
    print("The percent of heart attacks in the data: %0.2f%%" % (Heart_Attack_mean * 100))
    
    #create the Pie Chart
    plot_data = [Heart_Attack_mean, No_HA_Probability]
    labels = ["Heart Attack", "No Heart Attack"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.pie(plot_data, labels = labels, autopct = "%1.1f%%", startangle = 90)
    ax.set_title("Heart Attack Ratio in Data")
    plt.show()

def getGender():
    #Get the data for females with heart attacks
    Female_HA = data[(data.sex == 1) & (data.attack == 1)]
    #Now calculate the percentage
    Female_Percent = (len(Female_HA) / len(data[data["attack"] == 1])) * 100
    Male_Percent = 100 - Female_Percent     
    #Print Statements
    print("Male heart attacks: %0.2f%%" % Male_Percent)
    print("Female heart attacks: %0.2f%%" % Female_Percent)
    
    #Creat the Pie Chart 
    plot_data = [Male_Percent, Female_Percent]
    labels = ["Male", "Female"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.pie(plot_data, labels = labels, autopct = "%1.2f%%", startangle = 90)
    ax.set_title("Heart Attack Ratio between Male & Female")
    plt.show()

    
getHeartAttack()
getGender()
