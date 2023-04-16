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


def getEKG():
    print('EKG type:')
    print('    0 - EKG was normal')
    print('    1 - EKG had an abnormalitiy with the ST-T Wave')
    print('    2 - EKG had an abnormality in the Left ventricular')
    #find percentages
    EKG0 = data[(data.restecg == 0) & (data.attack == 1)]
    EKG0count = data[(data.restecg == 0)]
    EKG0percent = (len(EKG0) / len(EKG0count)) * 100
    
    EKG1 = data[(data.restecg == 1) & (data.attack == 1)]
    EKG1count = data[(data.restecg == 1)]
    EKG1percent = (len(EKG1) / len(EKG1count)) * 100
    
    EKG2 = data[(data.restecg == 2) & (data.attack == 1)]
    EKG2count = data[(data.restecg == 2)]
    EKG2percent = (len(EKG2) / len(EKG2count)) * 100
    
    #print statements
    print('Resting EKG Type 0 had this percent of heart attack: %0.2f%%' % EKG0percent)
    print('Resting EKG Type 1 had this percent of heart attack: %0.2f%%' % EKG1percent)
    print('Resting EKG Type 2 had this percent of heart attack: %0.2f%%' % EKG2percent)
    
    #make plot
    plot_data = [EKG0percent, EKG1percent, EKG2percent]
    labels = ["Normal", "Abnormal ST-T Wave", "Abnormal Left Ventricular"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.bar(labels, plot_data, color = "darkred")
    ax.set_xlabel('EKG Type')
    ax.set_ylabel('Percent')
    ax.set_title('Percent Heart Attack by Resting EKG')
    fig.savefig('EKG.png', bbox_inches = 'tight')
    plt.show()

getEKG()
    
def getExercise():
    #find percentages
    Exer = data[(data.exng == 1) & (data.attack == 1)]
    Exercount = data[(data.exng == 1)]
    Exerpercent = (len(Exer) / len(Exercount)) * 100

    
    NoExer = data[(data.exng == 0) & (data.attack == 1)]
    NoExercount = data[(data.exng == 0)]
    NoExerpercent = (len(NoExer) / len(NoExercount)) * 100
    
    #print statements
    print('Percent who Exercised and had a heart attack: %0.2f%%' % Exerpercent)
    print('Percent with No Exercise and had a heart attack: %0.2f%%' %  NoExerpercent)
    
    #make plot
    plot_data = [Exerpercent, NoExerpercent]
    labels = ["Yes", "No"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.bar(labels, plot_data, color = "purple")
    ax.set_xlabel('Exercise')
    ax.set_ylabel('Percent')
    ax.set_title('Percent Heart Attack if Exercising')
    fig.savefig('Exercise.png', bbox_inches = 'tight')
    plt.show()
 
getExercise()
    
