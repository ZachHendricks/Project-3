# Project-3
#Zach Hendricks

#Main
from Utilities import *

if __name__ == "__main__":
    getHeartAttack()
    getGender()
    getChestPain()
    getBloodSugar()
    getEKG()
    getExercise()
    
print("Heart Attack Analysis")

print("These are the factors related to heart attacks that are considered in this data:")
print("      1.  Hear Attack Percentage is calculated based on all data to see if the data is skewed.")
print("      2.  Gender Percent is used to distinguis between the heart attack rates in men and women.")
print("      3.  Four types of Chest Pain are considered, along with the associated heart attack problems.")
print("      4.  Fasting Blood Sugar levels are considered to determine possible coronary heart disease.")
print("      5.  EKG is considered, as it detects heart problems that can forecast chance of future heart attacks.")
print("      6.  Exercise is considered to see if likelihood of heart attack increases when physically active.")

#Utilities
#Zach Hendricks, Carter Barnes, Elise Rodriguez

import pandas as pd 

import matplotlib.pyplot as plt

data = pd.read_csv('heart.csv')

def getHeartAttack():
    #Get the heart attack probability
    Heart_Attack_mean = (data.attack.mean())
    #Take the probability and subtract 1
    No_HA_Probability = (1 - Heart_Attack_mean)
    #Print Statements
    print("The percent of no heart attacks in the data: %0.2f%%" % (No_HA_Probability * 100))
    print("The percent of heart attacks in the data: %0.2f%%" % (Heart_Attack_mean * 100))
    
    #create the Pie Chart
    plot_data = [Heart_Attack_mean, No_HA_Probability]
    labels = ["Heart Attack", "No Heart Attack"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.pie(plot_data, labels = labels, autopct = "%1.2f%%", startangle = 90)
    ax.set_title("Heart Attack Ratio in Data")
    plt.show()
    fig.savefig("HeartAttack.png", bbox_inches = "tight")

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
    fig.savefig("Gender.png", bbox_inches = "tight")
    
def getChestPain():
    #Get the data for all of the different chest pains
    #CP 0
    Chest_Pain_0 = data[(data.cp == 0) & (data.attack == 1)]
    Chest_Pain_0_Count = data[(data.cp == 0)]
    CP_Pct_0 = (len(Chest_Pain_0) / len(Chest_Pain_0_Count))
    
    
    #CP1
    Chest_Pain_1 = data[(data.cp == 1) & (data.attack == 1)]
    Chest_Pain_1_Count = data[(data.cp == 1)]
    CP_Pct_1 = (len(Chest_Pain_1) / len(Chest_Pain_1_Count))
    
    
    #CP2
    Chest_Pain_2 = data[(data.cp == 2) & (data.attack == 1)]
    Chest_Pain_2_Count = data[(data.cp == 2)]
    CP_Pct_2 = (len(Chest_Pain_2) / len(Chest_Pain_2_Count))
    
    
    #CP3
    Chest_Pain_3 = data[(data.cp == 3) & (data.attack == 1)]
    Chest_Pain_3_Count = data[(data.cp == 3)]
    CP_Pct_3 = (len(Chest_Pain_3) / len(Chest_Pain_3_Count))

    #Joint Probability... this one took a while and a lot of research
    # Count the number of times each combination of chest pain type and heart attack status occurs
    joint_counts = data.groupby(['cp', 'attack']).size().unstack()

    # Divide by the total number of observations to get the joint probability
    joint_probs = joint_counts / joint_counts.values.sum()

    #Print Statements
    
    print("Chest Pain type:")
    print("     0 - typical chest pain")
    print("     1 - atypical chest pain")
    print("     2 - any pain that is not chest pain")
    print("     3 - asymptomatic")
    print("Chest Pain type 0 had this percent of heart attack: %0.2f%%" % (CP_Pct_0 * 100))
    print("Chest Pain type 1 had this percent of heart attack: %0.2f%%" % (CP_Pct_1 * 100))                
    print("Chest Pain type 2 had this percent of heart attack: %0.2f%%" % (CP_Pct_2 * 100))
    print("Chest Pain type 3 had this percent of heart attack: %0.2f%%" % (CP_Pct_3 * 100))
    
    #Bar Chart Percent Heart Attack by Pain Type
    plot_data = [(CP_Pct_0 * 100), (CP_Pct_1 * 100), (CP_Pct_2 * 100), (CP_Pct_3 * 100)]
    labels = ["Typical" , "Atypical", "Any Pain", "Asymptomatic"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.bar(labels, plot_data, color = "brown", width = 0.6)
    ax.set_xlabel("Pain Type")
    ax.set_ylabel("Percent Heartattack")
    ax.set_title("Percent Heart Attack by Pain Type")
    fig.savefig("ChestPainHeartAttack.png", bbox_inches = "tight")
    plt.show()
    
    #Second Bar Chart with the weird numbers
    labels = ["Typical", "Atypical", "Any Pain", "Asymptomatic"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.bar(labels, (joint_probs[1] * 100), color="blue", width=0.5)
    ax.set_xlabel("Pain Type")
    ax.set_ylabel("Percent Heart Attack")
    ax.set_title("Percent Pain Reported")
    fig.savefig("ChestPain.png", bbox_inches="tight")
    plt.show()
    
def getBloodSugar():
    #FIND %
    FBS0 = data[(data.fbs == 0) & (data.attack == 1)]
    FBS0count = data[(data.fbs ==0)]
    FBS0percentage = (len(FBS0) / len(FBS0count)) * 100
   
    FBS1 = data[(data.fbs == 1) & (data.attack == 1)]
    FBS1count = data[(data.fbs ==1)]
    FBS1percentage = (len(FBS1) / len(FBS1count)) * 100
   
    #print statement
    print('High blood sugar heart attack percentage: %0.2f%%' % FBS0percentage)
    print('Regular blood sugar heart attack percentage: %0.2f%%' %  FBS1percentage)
    
    #Bar chart
    plot_data = [FBS1percentage, FBS0percentage]
    labels = ["High", "Regular"]
    fig = plt.figure()
    ax = fig.add_axes([0,0,1,1])
    ax.bar(labels, plot_data, color = "green")
    ax.set_xlabel('Blood Sugar')
    ax.set_ylabel('Percent Heartattack')
    ax.set_title('Percent Heart Attack by Blood Sugar Level')
    fig.savefig("BloodSugar.png" , bbox_inches = "tight")
    plt.show()
    
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



#statements to print data                    
getHeartAttack()
getGender()
getChestPain()
getBloodSugar()
getEKG()
getExercise() 
    
