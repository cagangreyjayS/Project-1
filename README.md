
#include <iostream>

#include <string>

#include <vector>

#include <windows.h> 

#include <direct.h>  



using namespace std;



void listFiles();

void createDirectory();

void changeDirectory();

void mainMenu();


int main() {

    mainMenu(); 

    return 0;

}



void mainMenu() {

    int choice;

    do {

       

        system("cls");

        cout << "--- Directory Management System ---\n";

        cout << "[1] List Files\n";

        cout << "[2] Create Directory\n";

        cout << "[3] Change Directory\n";

        cout << "[4] Exit\n";

        cout << "-----------------------------------\n";

        cout << "Current Directory: " << _getcwd(NULL, 0) << "\n";

        cout << "Enter your choice: ";

        cin >> choice;


        switch (choice) {

            case 1:

                listFiles();

                break;

            case 2:

                createDirectory();

                break;

            case 3:

                changeDirectory();

                break;

            case 4:

                cout << "Exiting the program. Goodbye!\n";

                break;

            default:

                cout << "Invalid choice. Please try again.\n";

        }

        cout << "\nPress Enter to continue...";

        cin.ignore();

        cin.get();

    } while (choice != 4);

}


void listFiles() {

    int option;

    string input;


    system("cls"); 

    cout << "--- List Files Submenu ---\n";

    cout << "[1] List All Files\n";

    cout << "[2] List Files by Extension\n";

    cout << "[3] List Files by Pattern\n";

    cout << "--------------------------\n";

    cout << "Enter your choice: ";

    cin >> option;


    cout << "\nFiles in current directory:\n";


    WIN32_FIND_DATAA fileData;

    HANDLE hFind = FindFirstFileA("*", &fileData); 


    if (hFind == INVALID_HANDLE_VALUE) {

        cout << "Error: Could not open the current directory.\n";

        return;

    }


    do {

        string filename = fileData.cFileName;

        if (filename == "." || filename == "..") {

            continue;

        }


        switch (option) {

            case 1: 

                cout << "- " << filename << "\n";

                break;

            case 2: { 

                cout << "Enter file extension (e.g., .txt): ";

                cin >> input;

                size_t dot_pos = filename.find_last_of('.');

                if (dot_pos != string::npos && filename.substr(dot_pos) == input) {

                    cout << "- " << filename << "\n";

                }

                break;

            }

            case 3: { 

                cout << "Enter file pattern (e.g., mo): ";

                cin >> input;

                if (filename.find(input) == 0) { // Check if filename starts with pattern

                    cout << "- " << filename << "\n";

                }

                break;

            }

            default:

                cout << "Invalid option. Returning to main menu.\n";

                break;

        }

    } while (FindNextFileA(hFind, &fileData) != 0);


    FindClose(hFind); 

}



void createDirectory() {

    string dirName;

    system("cls");

    cout << "Enter directory name: ";

    cin >> dirName;


    if (_mkdir(dirName.c_str()) == 0) {

        cout << "Directory \"" << dirName << "\" created successfully.\n";

    } else {

        cout << "Error: Could not create directory. It may already exist or you lack permissions.\n";

    }

}



void changeDirectory() {

    int option;

    string customPath;


    system("cls");

    cout << "--- Change Directory Submenu ---\n";

    cout << "[1] Move to Parent Directory\n";

    cout << "[2] Enter Custom Path\n";

    cout << "------------------------------\n";

    cout << "Enter your choice: ";

    cin >> option;


    char currentDir[FILENAME_MAX];

    _getcwd(currentDir, sizeof(currentDir));  


    string newPath;

    bool success = false;


    switch (option) {

        case 1: // Move to parent directory

            newPath = string(currentDir) + "\\..";

            if (_chdir(newPath.c_str()) == 0) {

                success = true;

            }

            break;

        case 2: // Enter a custom path

            cout << "Enter custom path (e.g., C:\\Users): ";

            cin.ignore(); // Clear the buffer

            getline(cin, customPath);

            if (_chdir(custo


