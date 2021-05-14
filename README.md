# password_manager
from getpass import getpass
import os
import time
import os.path


count = 0
passw = ""
options = ["d: Titel löschen", "p: Hallo Welt", "t: Titel anzeigen", "a: Titel hinzufügen", "e: Programm beenden", "c: Master-Passwort ändern"]
os.chdir(r"C:\Users\felix\Desktop")
pfad = os.getcwd()
#print(pfad)




#Anzeigen der Optionen:
def optionen():

    print("\nWas möchten Sie tun?\t")
    print("")
    for option in options:
        print(option)

    Q =input()

    #Aussuchen der Optionen:

    #Titel löschen
    if Q == "d":
        Delete()

    #Test Option
    elif Q == "p":
        print("Hallo Welt")
        optionen()
    #Alle Titel auflisten:

    elif Q == "t":
        path = os.chdir(pfad+"\\PManager")
        print("")
        print("Titel:")
        for titel in os.listdir(path):
            print(titel)
        Back()

    #Hinzufügen von Titeln:
    elif Q == "a":
        AddTitel()
        optionen()

    #Programm beenden:
    elif Q == "e":
        print("Das Programm wird beendet.")
        exit()

    #Ändern des MWP
    elif Q == "c":
        chMWP()

    else:
        print("Eingabe stimmt mit keiner Option überein.")
        time.sleep(1.5)
        optionen()

#Erstellen von Ordner, in dem Passwörter gespeichert sind:

def right():

    listA = os.makedirs("PManager") #erstellt Ordner PManager


#Masterpasswort Abfrage:

def Pass(passw,count,newpassw):


    if passw==newpassw:

        return True
    else:

        if count < 2 :
            print("Falsches Passwort. Sie haben noch",2 - count,"Versuch(e).")
            passw=getpass("Bitte erneut eingeben:\t")
            count=count+1
            if Pass(passw,count,newpassw) == True:
                return True
        else :
            print("Zu viele Versuche! Das Programm wird beendet.")

            return False


#Löschen von Titel:
def Delete():

    path = os.listdir(pfad+"\\PManager")#-->
    print (path)                        #--> Listet alle Titel in PMAnager auf
    print("")
    os.chdir(pfad+"\\PManager") #Ändert Dateizugriff, sodass Titel bearbeitet werden können
    delTitel = input("Welcher Titel soll gelöscht werden?('e' zum zurückkehren)\t")+".txt"    #Auswahl des Titels + sparen des .txt
    if delTitel =="e.txt":   #Zurück ins Menü
        optionen()
    elif os.path.exists(delTitel):  #Überprüft, ob eingabe existiert
        if input("Sind Sie sicher?y/n\t") == "y":
            os.remove(delTitel) #Löscht titel
            print("Titel erfolgreich gelöscht.")
            optionen()
        else:
            optionen()
    else:
        print("Dieser Titel existiert nicht.\n")
        Delete()


def AddTitel():
    name = input("Neuer Titel('e' um zurückzukehren.):\t")
    path = os.getcwd()
    if name == "e":
        optionen()


    elif path.endswith("PManager") == False:
        newPath = os.chdir(path + "/PManager")
        newTitel = open(name + ".txt", "w+")
        newTitel.close()
        chTitel(name)
    else:
        newTitel = open(name + ".txt", "w+")
        newTitel.close()
        chTitel(name)
def Back():
    print("")
    inp = input("Geben Sie 'e' ein, um zurückzukehren.\t")
    if inp == "e":
        optionen()
    else:
        print("Eingabe stimmt nicht überein.")
        Back()


def chTitel(name):

    k = input("Neues Passwort anlegen? y/n\t")
    if k == "y":
        Tipa = open(name +".txt", "w")
        Tipa.write(input("Neues Passwort:\t"))
        print("Neues Passwort angelegt.")
       # Tipa = open(name +".txt", "r")
       # n = Tipa.read()
        #print(n)
    elif k == "n":
        optionen()
    else:
        print("Eingabe stimmt nicht überein.")
        chTitel(name)

def chMWP():
    inp = input("Wollen Sie wirklich ihr Master-Passwort ändern?y/n\t")
    if inp == "y":
        neu = input("Neues Passwort:\t")
        neu2 = input("Neues Passwort wiederholen:\t")
        if neu == neu2:
            os.chdir(pfad+"\\PManager")
            w = open("MasterPW.txt", "w")
            w.write(neu)
            print("Master-Passwort erfolgreich geändert.")
            optionen()
        else:
            print("Eingaben stimmen nicht überein.")
            chMWP()


    elif inp == "n":
        optionen()
    else:
        print("Eingabe stimmt nicht überein.")
        chMWP()


#Anmeldung:

if os.path.exists("PManager") != True:  #Überprüft, ob Ordner PManager existiert. Wenn nicht --> erstmaliges Starten --> Masterpw Eingabe

        print("Willkommen.")
        right()
        pw = input("Geben Sie ihr Masterpasswort ein:\t")
        newpassw = open (pfad+"\\PManager\\MasterPW.txt","w+")
        newpassw.write(pw)

        optionen()
else:   #Normale Anmeldung ab 2. Starten des Programms

    np = open(pfad + "\\PManager\\MasterPW.txt", "r")   #öffnet Datei mit Mpw
    newpassw = np.read()    #Liest das Mpw aus der Datei
    passw = getpass("Bitte geben Sie ihr Passwort ein:\t")

    if Pass(passw,count,newpassw) == True:

        print("Willkommen.")
        optionen()
    else:

        exit()


