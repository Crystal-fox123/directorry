#include <iostream>
#include <filesystem>
#include <regex>
#include <string>

using namespace stsd;
namespace fs = filesystem;

void listAllFiles(const fs::path& directory) {
   cout << "Files in directory: " << directory.string() << endl;
    for (const auto& entry : fs::directory_iterator(directory)) {
       cout << (entry.is_directory() ? "[DIR] " : "[FILE] ")
                  << entry.path().filename().string() << endl;
    }
}

void listFilesByExtension(const fs::path& directory, const std::string& extension) {
   cout << "Files with extension " << extension << " in directory: " << directory.string() << endl;
    for (const auto& entry : fs::directory_iterator(directory)) {
        if (entry.path().extension() == extension) {
            cout << entry.path().filename().string() << endl;
        }
    }
}

void listFilesByPattern(const fs::path& directory, const std::string& pattern) {
   regex regexPattern(pattern);
    cout << "Files matching pattern " << pattern << " in directory: " << directory.string() << endl;
    for (const auto& entry : fs::directory_iterator(directory)) {
        if (regex_match(entry.path().filename().string(), regexPattern)) {
            cout << entry.path().filename().string() << endl;
        }
    }
}

void listFilesMenu(const fs::path& directory) {
    cout << "1. List all files\n";
   cout << "2. List files by extension\n";
    cout << "3. List files by pattern\n";
    cout << "Select an option: ";

    int choice;
    cin >> choice;
    switch (choice) {
        case 1:
            listAllFiles(directory);
            break;
        case 2: {
           string extension;
            cout << "Enter the file extension (e.g., .txt): ";
            cin >> extension;
            listFilesByExtension(directory, extension);
            break;
        }
        case 3: {
            string pattern;
            cout << "Enter the pattern (e.g., moha*.*): ";
            cin >> pattern;
            listFilesByPattern(directory, pattern);
            break;
        }
        default:
            cout << "Invalid option. Please try again." << std::endl;
    }
}

void createDirectory(const fs::path& directory) {
    if (fs::create_directory(directory)) {
       cout << "Directory created: " << directory.string() << std::endl;
    } else {
        cout << "Failed to create directory or it already exists." << std::endl;
    }
}

void changeDirectory(fs::path& currentPath) {
    cout << "1. Move to the parent directory\n";
    cout << "2. Move to the root directory\n";
    cout << "3. Move to a specific directory\n";
    cout << "Select an option: ";

    int choice;
    cin >> choice;
    switch (choice) {
        case 1:
            currentPath = currentPath.parent_path();
            break;
        case 2:
            currentPath = fs::path("/");
            break;
        case 3: {
            string dirName;
           cout << "Enter directory name to change to: ";
            cin >> dirName;
            fs::path newPath = currentPath / dirName;
            if (fs::exists(newPath) && fs::is_directory(newPath)) {
                currentPath = newPath;
            } else {
               cout << "Directory does not exist." << endl;
            }
            break;
        }
        default:
            cout << "Invalid option. Please try again." << endl;
    }
   cout << "Current Directory: " << currentPath.string() << endl;
}

int main() {
    fs::path currentPath = fs::current_path();

    while (true) {
        cout << "\nDirectory Management System\n";
        cout << "Current Directory: " << currentPath.string() << std::endl;
        cout << "1. List Files\n";
        cout << "2. Create Directory\n";
        cout << "3. Change Directory\n";
        cout << "4. Exit\n";
        cout << "Select an option: ";

        int choice;
        cin >> choice;

        switch (choice) {
            case 1:
                listFilesMenu(currentPath);
                break;
            case 2: {
                string dirName;
                cout << "Enter directory name to create: ";
                cin >> dirName;
                createDirectory(currentPath / dirName);
                break;
            }
            case 3:
                changeDirectory(currentPath);
                break;
            case 4:
                cout << "Exiting..." << endl;
                return 0;
            default:
                cout << "Invalid option. Please try again." << endl;
        }
    }
}
