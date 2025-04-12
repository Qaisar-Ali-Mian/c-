# c++
//simple library management system
//you can add delete_funtion to it

//code start here
#include <iostream>
#include <string>
#include <fstream>
#include<cstdlib>
#include <windows.h>
using namespace std;
class library
{
    string Name;
    int Id;
    string Author;
    int Quantity;
public:
    library():Name(""),Id(0),Author(""),Quantity(0) {}
    void setname(string name)
    {
        Name = name;
    }
    void set_id(int id)
    {
        Id = id;
    }
    void set_author(string author)
    {
        Author = author;
    }

    void set_quantity(int quantity)
    {
        Quantity = quantity;
    }

    string getname(void)
    {
        return Name;
    };

    string get_author(void)
    {
        return Author;
    }
    int get_id(void)
    {
        return Id;
    }
    int get_quantity(void)
    {
        return Quantity;
    }

};
void add_book(library *arrofbook)
{
    int choice;
    bool exit = false;
    while(!exit)
    {
        cout<<"************************************"<<endl;
        cout<<"1: to add book"<<endl;
        cout<<"2: to close"<<endl;
        cout<<"enter choice"<<endl;
        cin>>choice;
        if(choice == 1)
        {
            string bookname,author;
            int id,quantity;
            cout<<"enter name"<<endl;
            cin>>bookname;
            (*arrofbook).setname(bookname);

            cout<<"enter author"<<endl;
            cin>>author;
            (*arrofbook).set_author(author);

            cout<<"enter id"<<endl;
            cin>>id;
            (*arrofbook).set_id(id);

            cout<<"enter quantity"<<endl;
            cin>>quantity;
            (*arrofbook).set_quantity(quantity);
            ofstream out("D:\\projects\\projectinc++\\data\\bookdetail.txt", ios::app);
            if(!out)
            {
                cout<<"  error: file cant open"<<endl;

            }
            else
            {
                out<<(*arrofbook).getname()<<","<<(*arrofbook).get_author()<<","<<(*arrofbook).get_id()<<","<<(*arrofbook).get_quantity()<<"\n";
                MessageBox(NULL, "Book added successfully!", "Library Info", MB_OK);
            }
        }
        else
            exit = true;
        Sleep(3000);

    }

}
void viewbook(void)
{
    ifstream file("D:\\projects\\projectinc++\\data\\bookdetail.txt");
    if(!file)
    {
        cout<<"file not found"<<endl;
    }
    else
    {
        string line;
        while(getline(file,line))
        {
            cout<<line<<endl;
        }
        file.close();
        Sleep(3000);
        cout<<endl<<endl;
    }
}
void search_id(void)
{
    ifstream file("D:\\projects\\projectinc++\\data\\bookdetail.txt");
    if (!file)
    {
        cout << "File not found!" << endl;
        return;
    }

    int search_id;
    cout << "Enter the ID of the book you want to search: ";
    cin >> search_id;

    string line;
    bool found = false;

    while (getline(file, line))
    {
        string name = "", author = "", id_str = "", quantity_str = "";
        int id = 0, quantity = 0;
        int part = 0;
        string temp = "";

        for (int i = 0; i < line.length(); i++)
        {
            if (line[i] == ',')
            {
                if (part == 0) name = temp;
                else if (part == 1) author = temp;
                else if (part == 2) id_str = temp;
                temp = "";
                part++;
            }
            else
            {
                temp += line[i];
            }
        }

        quantity_str = temp; // last part

        try
        {
            id = stoi(id_str);
            quantity = stoi(quantity_str);
        }
        catch (...)
        {
            continue; // skip if there's an error
        }

        if (id == search_id)
        {
            cout << "\n Book Found!" << endl;
            cout << "Name: " << name << endl;
            cout << "Author: " << author << endl;
            cout << "ID: " << id << endl;
            cout << "Quantity: " << quantity << endl;
            found = true;
            break;
        }
    }

    file.close();

    if (!found)
    {
        cout << "\n Book with ID " << search_id << " not found." << endl;
    }
}
int main()
{
    library b1;
    bool exit = false;
    while(!exit)
    {
        int choice;

        cout<<"   welcome to public library       "<<endl;
        cout<<"-----------------------------------"<<endl;
        cout<<"   press 1 to add new book         "<<endl;
        cout<<"   press 2 search book by id       "<<endl;
        cout<<"   press 3 to view all book        "<<endl;
        cout<<"   press 4 to delete a book        "<<endl;
        cout<<"   press 0 to exit                 "<<endl;
        cin>>choice;
        system("cls");

        if(choice == 1)
        {
            add_book(&b1);
        }
        else if(choice ==  2)
        {
            search_id();
        }
        else if(choice == 3)
        {
            viewbook();
        }
        else
        {
            exit = true;
        }
    }
    return 0;
}

