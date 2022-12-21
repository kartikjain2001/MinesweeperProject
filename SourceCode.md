```cpp
#include <iostream>
#include <stdlib.h>
#include <vector>
#include <limits>
using namespace std;
class Board{
    public:
        vector<int> hiddenVector;
        vector<int> displayVector;
        void displayBoard(vector<int> displayVector);
        bool checkForWin(vector<int> displayVector,vector<int> hiddenVector);
        void clearWhiteSpace(vector<int>& displayVector,vector<int>& hiddenVector);
        void inputChoices(int x, int y,int z){ // Preset boards
            rows = x;
            cols = y;
            mines = z;
        }
        void inputChoices();
        void createBoard(vector<int>& hiddenVector,vector<int>& displayVector);
        void uncoverAllMines(vector<int>& hiddenVector,vector<int>& displayVector);
        void play(vector<int>& hiddenVector,vector<int>& displayVector);
        int getRows(){
                return rows;
        }
        int getColumns(){
                return cols;
        }
        int getMines(){
                return mines;
        }
        int getPlayerX(){//Players Row
        int x = 0;
        while(cout << "Please input a row: " && !(cin >> x) || (x < 1 || x > rows)){
            cout << "Not a valid input. Try again." << endl;
            cin.clear(); // reset failbit
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        return x;
}
        int getPlayerY(){// Players Column
        int y = 0;
        while(cout << "Please input a column: " && !(cin >> y) || (y < 1 || y > cols)){
            cout << "Not a valid input. Try again." << endl;
            cin.clear(); // reset failbit
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        return y;
    }
    private:
        int rows;
        int cols;
        int mines;
};
//*** End of Board Definition ***//
void Board::displayBoard(vector<int> displayVector){
    int countc = 1;
    int countr = 1;
    int n = 0;
    cout << "    ";
    for(int j = 0; j < cols;j++){
        if(j <= n){
            cout << "  " << countc;
        }
        else{
            cout << " " << countc;
        }
        n += 3;
        countc++;
    }
    cout << endl << "   |-";
    for(int i = 0; i < n;i++){
        cout << "-";
    }
    cout << "-|" << endl;
    for(int r = 0; r < rows; r++){
        for (int c = -1; c < cols; c++){
            if(c == -1 && r < 9){
                cout << countr << "   | ";
                countr++;
                c++;
            }
            else if(c == -1){
                cout << countr << "| ";
                countr++;
                c++;
            }

            int temp = displayVector.at((r * rows + c));
            if(temp == -3){ // Covered space
                cout << "C";
            }
            else if(temp == 9){ // Flag
                cout << "F";
            }
            else if(temp == -2){ // Question mark
                cout << "?";
            }
             else if(temp == -1){ // Mine
                cout << "*";
            }
            else if(temp == 0){ // Uncovered blank space
                cout << " ";
            }
            else{ // All other numbers
                cout << temp;
            }
            if (c != cols-1){
                cout << "  ";
            }
            else{
                cout << " |";
            }
        }
        cout << endl;
    }
    cout << "   |_";
    for(int i = 0; i < n;i++){
        cout << "_";
    }
    cout << "_|" << endl;
}
bool Board::checkForWin(vector<int> hiddenVector,vector<int> displayVector){
    int tally = 0;
    for(int i = 0; i < hiddenVector.size();i++)
    {
        if(hiddenVector.at(i) == -1 && displayVector.at(i) == 9)
        tally++;
    }
    if(tally == mines)
    return true;
    else
    return false;
}
void Board::clearWhiteSpace(vector<int>& hiddenVector,vector<int>& displayVector){
for(int i = 0; i < hiddenVector.size()-mines;i++)
{
        for(int a = 0; a < hiddenVector.size();a++)
         {
             if(displayVector.at(a) == 0){
                     if(a < cols){
                        if(a == 0){
                         displayVector.at(a+1)=hiddenVector.at(a+1);
                          displayVector.at(a + cols)=hiddenVector.at(a + cols);
                          displayVector.at(a+cols + 1)=hiddenVector.at(a+cols + 1);
                        }
                     else if(a == cols - 1)     //top right corner
                     {
                         displayVector.at(a - 1)=hiddenVector.at(a - 1);
                         displayVector.at(a + cols)=hiddenVector.at(a + cols);
                         displayVector.at(a+cols - 1)=hiddenVector.at(a+cols - 1);
                     }
                     else // any others in the first row
                     {
                         displayVector.at(a - 1)=hiddenVector.at(a - 1);
                         displayVector.at(a + cols)=hiddenVector.at(a + cols);
                         displayVector.at(a + cols - 1)=hiddenVector.at(a+ cols - 1 );
                         displayVector.at(a + 1)=hiddenVector.at(a+1);  // top left corner
                         displayVector.at(a+1+cols)=hiddenVector.at(a+1+cols);
                    }}
                 else if(a%cols == 0)// first column
                 {
                  if( a != (cols * rows - cols)) // first column excluding the bottom left corner
                  {
                         displayVector.at(a+1)=hiddenVector.at(a+1);
                         displayVector.at(a + cols)=hiddenVector.at(a + cols);
                         displayVector.at(a-cols + 1)=hiddenVector.at(a-cols + 1);
                         displayVector.at(a+1+cols)=hiddenVector.at(a+1+cols);
                         displayVector.at(a-cols)=hiddenVector.at(a-cols);
                  }
                  else
                  {
                         displayVector.at(a-cols + 1)=hiddenVector.at(a-cols + 1);
                         displayVector.at(a+1)=hiddenVector.at(a+1);  // bottom left corner
                         displayVector.at(a-cols)=hiddenVector.at(a-cols);
                  }
                 }
                     else if(a%(cols) == cols - 1)
                     {                          // right side
                        if(a == (cols * rows) - 1) // bottom right corner
                        {
                         displayVector.at(a-cols - 1)=hiddenVector.at(a-cols - 1);
                         displayVector.at(a- 1)=hiddenVector.at(a - 1);
                         displayVector.at(a - 1)=hiddenVector.at(a - 1);
                        }
                        else
                        {
                         displayVector.at(a - 1)=hiddenVector.at(a- 1);
                         displayVector.at(a+ cols)=hiddenVector.at(a+cols);
                         displayVector.at(a+cols - 1)=hiddenVector.at(a+cols - 1);
                         displayVector.at(a- 1-cols)=hiddenVector.at(a- 1-cols);
                         displayVector.at(a-cols)=hiddenVector.at(a-cols);
                        }
                     }
                     else if(a > (cols * (rows - 1)) && a < (rows * cols - 1))
                     {
                         //bottom row
                         displayVector.at(a-cols - 1)=hiddenVector.at(a-cols - 1);
                         displayVector.at(a - 1)=hiddenVector.at(a - 1);
                         displayVector.at(a+1-cols)=hiddenVector.at(a+1-cols);
                         displayVector.at(a+1)=hiddenVector.at(a+1);
                         displayVector.at(a-cols)=hiddenVector.at(a-cols);
                     }
                     else if(a != cols - 1 &&a < (rows * (cols- 1))){       //every other space
                         displayVector.at(a-cols - 1)=hiddenVector.at(a-cols - 1);
                         displayVector.at(a-1)=hiddenVector.at(a-1);
                         displayVector.at(a+1-cols)=hiddenVector.at(a+1-cols);
                         displayVector.at(a+1)=hiddenVector.at(a+1);
                         displayVector.at(a-cols)=hiddenVector.at(a-cols);
                         displayVector.at(a+cols - 1)=hiddenVector.at(a+cols - 1);
                         displayVector.at(a+cols+1)=hiddenVector.at(a+cols+1);
                         displayVector.at(a+cols)=hiddenVector.at(a+cols);}
             }}
         }
    }
void Board::inputChoices() {// Checks for valid entry
    rows=cols=mines=1;
    while(cout << "Enter the number of rows and columns(max 40): " && !(cin >> rows) || (rows > 40 || rows < 2)) {
        cout << "Not a valid input. Try again." << endl;
        cin.clear(); // reset failbit
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
    }
    cols = rows;
    cout<<"Enter the number of mines(max "<<rows*cols-1<<"): "<<endl;
    cin>>mines;
    while(cout << "Enter the number of mines(max "<<rows*rows-1<<"): " && !(cin >> mines) || (mines>rows*rows-1) || (mines < 1)) {
        cout << "Not a valid input. Try again." << endl;
        cin.clear(); // reset failbit
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
    }
}
void Board::createBoard(vector<int>& hiddenVector,vector<int>& displayVector){
    for(int i = 0; i<rows*cols;i++) // Sets whole board to 0;
    {
        hiddenVector.push_back(0);
        displayVector.push_back(-3);
    }
    int minesLeft = mines; // Sets the mines
    for(int j = 0; j<rows*cols;j++)
    {
        int v1 = rand() % (rows*cols);
        if((v1 <= mines || hiddenVector.size()-1-j == minesLeft) && minesLeft > 0) // If mines left is == spaces left, then set all remaining spaces to mines
        {
            hiddenVector.at(j) = -1;
            minesLeft--;
        }
    }
     for(int a = 0; a < hiddenVector.size();a++) // Sets all the numbers
         {
             if(hiddenVector.at(a) != -1)
             {
                 if(a<cols)
                 {
                     if(a == 0)
                     {
                         if(hiddenVector.at(a+1)== -1)       // top left corner
                         hiddenVector.at(a)++;
                         if(hiddenVector.at(a + cols)==-1)
                          hiddenVector.at(a)++;
                         if(hiddenVector.at(a+cols + 1) == -1)
                          hiddenVector.at(a)++;
                     }
                     else if(a == cols -1)     //top right corner
                     {
                        if(hiddenVector.at(a-1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a + cols)==-1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+cols - 1) == -1)
                         hiddenVector.at(a)++;
                     }
                     else // any others in the first row
                     {
                        if(hiddenVector.at(a-1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a + cols)==-1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+cols - 1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1+cols)== -1)       // top left corner
                         hiddenVector.at(a)++;
                    }
                 }
                 else if(a%cols == 0)// first column
                 {
                  if( a != (cols * rows - cols)) // first column excluding the bottom left corner
                  {
                        if(hiddenVector.at(a+1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a + cols)==-1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-cols + 1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1+cols)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-cols)== -1)
                         hiddenVector.at(a)++;
                  }
                  else
                  {
                        if(hiddenVector.at(a-cols + 1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-cols)== -1)       // bottom left corner
                         hiddenVector.at(a)++;
                  }
                 }
                     else if(a%(cols) == cols - 1)
                     {                          // right side
                        if(a == (cols * rows) - 1) // bottom right corner
                        {
                        if(hiddenVector.at(a-cols - 1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-1-cols)== -1)
                         hiddenVector.at(a)++;
                        }
                        else
                        {
                        if(hiddenVector.at(a-1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a + cols)==-1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+cols -1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-1-cols)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-cols)== -1)
                         hiddenVector.at(a)++;
                        }
                     }
                     else if(a > (cols * (rows - 1)) && a < (rows * cols-1))
                     {
                         //bottom row
                        if(hiddenVector.at(a-cols - 1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1-cols)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-cols)== -1)
                        hiddenVector.at(a)++;
                     }
                     else if(a != cols -1 &&a < (rows * (cols-1))){       //every other space
                         if(hiddenVector.at(a-cols - 1) == -1) //
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1-cols)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a-cols)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+cols - 1) == -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+cols+1)== -1)
                         hiddenVector.at(a)++;
                        if(hiddenVector.at(a+cols)== -1)
                         hiddenVector.at(a)++;}
             }
         }
}
void Board::uncoverAllMines(vector<int>& hiddenVector,vector<int>& displayVector){
    for(int i = 0; i < hiddenVector.size();i++)
    {
        if(hiddenVector.at(i)== -1)
        displayVector.at(i)=-1;
    }
}
void Board::play(vector<int>& hiddenVector,vector<int>& displayVector){
    int x;
    int y;
    int play = 1;
    inputChoices();

        if(play == 0){
        }
        int flagCount = getMines();
        createBoard(hiddenVector,displayVector);
        int game = 1;
        while(game == 1){
            displayBoard(displayVector); //display vector in final
            cout<<endl;
            cout<<"Flags Left to place: "<<flagCount<<endl;
            cout<<endl;
            int choice = -1;
            while(choice == -1){
            cout<<"Enter coordinates(row,column): "<<endl;
            x = getPlayerX()-1;
            y = getPlayerY()-1;
            cout<<"\n1. Reveal tile"<<endl;
            cout<<"2. Place flag(place flag again to remove)"<<endl;                // tile becomes 9
            cout<<"3. Place question mark (?)"<<endl;  // tile becomes -2
            cout<<"4. Select new Tile"<<endl;
            cout<<"5. Exit game"<<endl;
            cout<<"What would you like to do here? "<<endl;
            //cin>>choice;
            while( !(cin >> choice) || (choice > 5) || choice < 1){
                cout << "Not a valid input. Try again." << endl;
                cin.clear(); // reset failbit
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
            }
            //if(choice > 5 || choice < 1) choice = -1;
            }
            switch (choice){
            case 1:
                if(hiddenVector.at(x*cols + y) == -1){
                    uncoverAllMines(hiddenVector,displayVector);
                    displayBoard(displayVector);
                    cout << "***********************************" << endl;
                    cout<<"You lose! :("<<endl;
                    cout << "***********************************" << endl;
                    displayVector.clear();
                    hiddenVector.clear();
                    game = 0;
                }
                else if(hiddenVector.at(x*cols + y) == 0){
                    displayVector.at(x*cols + y) = hiddenVector.at(x*cols + y);
                    clearWhiteSpace(hiddenVector,displayVector);
                }
                else{
                    displayVector.at(x*cols + y) = hiddenVector.at(x*cols + y);
                }
                break;
                break;
            case 2:
                if( displayVector.at(x*cols + y) != 9)
                displayVector.at(x*cols + y) = 9;
                else
                 displayVector.at(x*cols + y) = -3;
                if(hiddenVector.at(x*cols + y) == -1)
                flagCount--;
                break;
            case 3:
               displayVector.at(x*cols + y) = -2;
                break;
            case 4:
                choice = -2;
                break;
            case 5://exit game
                play = 0;
                cout << "***********************************" << endl;
                cout << "Thanks for Playing Minesweeper :)" << endl;
                cout << "***********************************" << endl;
                break;
            default:
                choice = -1;
                break;
            }
            if(checkForWin(hiddenVector,displayVector))
            {
                cout << "***********************************" << endl;
                cout<<"You Won!"<< endl;
                cout << "***********************************" << endl;
                displayVector.clear();
                hiddenVector.clear();
                game = 0;
                break;
            }
            if(play == 0){
                break;
            }
        }
    }

int main()
{
    Board b1;
    b1.play(b1.hiddenVector,b1.displayVector);
    return 0;
}
```
