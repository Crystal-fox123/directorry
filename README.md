#include <iostream>
#include <direct.h>
#include <string>
using namespace std;

void File_list();
void Direc_tory();
void ChangeDIR();

int main() {
    int Options;
    while (true) {
        cout << "      Main Menu:     " << endl;
        cout << "--------------------------------"<< endl;
        cout << "1. To Display List of Files"<< endl;
        cout << "2. To Create New Directory"<< endl;
        cout << "3. To Change the Working Directory"<< endl;
        cout << "4. Exit Program"<< endl;
        cout << "Enter Number: ";
        cin >> Options;

        switch (Options) {
            case 1:
                File_list();
                break;
            case 2:
                Direc_tory();
                break;
            case 3:
                ChangeDIR();
                break;
            case 4:
                return 0;
            default:
                cout << "Invalid. PLEASE TRY AGAIN."<< endl;
        }
    }
    return 0;
}

void File_list() {
    int Options;
    cout << "     LIST FILE DETAIL:" << endl;
    cout << "--------------------------------------" << endl;
    cout << "1. List All Files"<< endl;
    cout << "2. List of Extension Files"<< endl;
    cout << "3. List of Name Wise"<< endl;
    cout << "Enter Number: ";
    cin >> Options;


	if(Options == 1){
		cout<< " List of all files" << endl;
		system("dir");
}
	else if (Options == 2){
		string ext;
		cout << " Enter file extension : ";
		cin>> ext;
		system(("dir *." + ext).c_str());
}
	else if(Options == 3){
		string pattern;
		cout<< " Enter file name pattern:";
		cin >> pattern;
		system(("dir " + pattern).c_str());
}
	else {
		 cout << "Invalid. Please TRY AGAIN."<< endl;
}
		
}


void Direc_tory() {
    string dir;
    cout << "Directory name: ";
    cin >> dir;

    if (_mkdir(dir.c_str()) == 0) {
        cout << "Directory created." << endl;
    } else {
        cout << "Error creating directory. It may already exist or be invalid." << endl;
    }
}

void ChangeDIR() {
    int command;
    cout << "Change Directory Menu:" << endl;
    cout << "1. Move one step back." << endl;
    cout << "2. Move to the root directory." << endl;
    cout << "3. Move to a specific directory provided by the user." << endl;
    cout << "Enter your choice: ";
    cin >> command;

    switch (command) {
        case 1:
            if (_chdir("..") == 0) {
                cout << "Moved to parent directory." << endl;
            } else {
                cout << "Error moving to parent directory." << endl;
            }
            break;
        case 2:
            if (_chdir("\\") == 0) {
                cout << "Moved to root directory." << endl;
            } else {
                cout << "Error moving to root directory." << endl;
            }
            break;
        case 3: {
            string dir;
            cout << "Directory name: ";
            cin >> dir;
            if (_chdir(dir.c_str()) == 0) {
                cout << "Directory changed successfully." << endl;
            } else {
                cout << "ERROR changing directory. It may not exist." << endl;
            }
            break;
        }
        default:
            cout << "Invalid, TRY AGAIN." << endl;
    }
    
}

