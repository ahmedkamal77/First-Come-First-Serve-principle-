// First-Come First-Serve.cpp : This file contains the 'main' function. Program execution begins and ends there.
//

#include <iostream>
#include <list>
using namespace std;

#define Tracks 5
#define Sectors 5

int main()
{
    list <int> requests;
    list<int>::iterator it = requests.begin();  //create iterator(pointer) to point to the begining of the requests list
    int n = 0, x = -1;
    int seekTime = 5, searchTime = 1, transferTime = 1, totalSeek = 0, totalSearch = 0, totalTransfer = 0;
    int track = 0, sector = 0;
    cout << "Enter the number of requests:";
    cin >> n;
    cout << endl;
    cout << "Enter the order of requests e.g: 4 7" << endl;
    cout << "                                 3 1" << endl;
    cout << "                                 ..." << endl;

    for (int i = 0; i < (n*2); i++)
    {
        cin >> x;
        requests.insert(it, x);
    }
    it = requests.begin();


    for (int i = 0; i < n; i++)
    {
        //This algorithm assumes the movable-head at the begining of the sector
        totalSeek += (abs(*it - track))*seekTime;
        track = *it;
        it++;

        if (*it > sector)
            totalSearch += ((*it - sector) * searchTime);
        else if(sector > *it)
            totalSearch += ((Sectors - sector) + *it) * searchTime;

        sector = *it + 1; 
        if (sector == Sectors)   //Cylinder Rotation
            sector = 0;
        it++;
    }
    totalTransfer = n * transferTime;
    
    cout << "Total seek     = " << totalSeek <<" ms" << endl;
    cout << "Total search   = " << totalSearch <<" ms" << endl;
    cout << "Total transfer = " << totalTransfer <<" ms" << endl;
    cout << "Total Time     = " << totalSeek + totalSearch + totalTransfer << " ms" << endl;
}


