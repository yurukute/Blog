---
title: "TheLift"
date: 2022-06-06T00:45:12+07:00
tags: []
#ShowToc: true
TocOpen: true
---
# Problem
Source: [Codewars](https://www.codewars.com/kata/58905bfa1decb981da00009e)
### Synopsis
A multi-floor building has a Lift in it.

People are queued on different floors waiting for the Lift.

Some people want to go up. Some people want to go down.

The floor they want to go to is represented by a number (i.e. when they enter the Lift this is the button they will press)
```text
    BEFORE (people waiting in queues)            AFTER (people at their destinations)
                   +--+                                          +--+ 
  /----------------|  |----------------\        /----------------|  |----------------\
10|                |  | 1,4,3,2        |      10|             10 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 9|                |  | 1,10,2         |       9|                |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 8|                |  |                |       8|                |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 7|                |  | 3,6,4,5,6      |       7|                |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 6|                |  |                |       6|          6,6,6 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 5|                |  |                |       5|            5,5 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 4|                |  | 0,0,0          |       4|          4,4,4 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 3|                |  |                |       3|            3,3 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 2|                |  | 4              |       2|          2,2,2 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 1|                |  | 6,5,2          |       1|            1,1 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 G|                |  |                |       G|          0,0,0 |  |                |
  |====================================|        |====================================|
```
### Rules
#### Lift Rules
The Lift only goes up or down!

Each floor has both UP and DOWN Lift-call buttons (except top and ground floors which have only DOWN and UP respectively)

The Lift never changes direction until there are no more people wanting to get on/off in the direction it is already travelling

When empty the Lift tries to be smart. For example,
- If it was going up then it may continue up to collect the highest floor person wanting to go down
- If it was going down then it may continue down to collect the lowest floor person wanting to go up

The Lift has a maximum capacity of people

When called, the Lift will stop at a floor even if it is full, although unless somebody gets off nobody else can get on!

If the lift is empty, and no people are waiting, then it will return to the ground floor
#### People Rules
People are in "queues" that represent their order of arrival to wait for the Lift

All people can press the UP/DOWN Lift-call buttons

Only people going the same direction as the Lift may enter it

Entry is according to the "queue" order, but those unable to enter do not block those behind them that can

If a person is unable to enter a full Lift, they will press the UP/DOWN Lift-call button again after it has departed without them

### Kata Task
Get all the people to the floors they want to go to while obeying the Lift rules and the People rules

Return a list of all floors that the Lift stopped at (in the order visited!)

NOTE: The Lift always starts on the ground floor (and people waiting on the ground floor may enter immediately)

### I/O
#### Input
`queues` a list of queues of people for all floors of the building.

The height of the building varies

0 = the ground floor

Not all floors have queues

Queue index [0] is the "head" of the queue

Numbers indicate which floor the person wants go to

`capacity`: maximum number of people allowed in the lift

Parameter validation - All input parameters can be assumed OK. No need to check for things like:
- People wanting to go to floors that do not exist
- People wanting to take the Lift to the floor they are already on
- Buildings with < 2 floors
- Basements
#### Output
A list of all floors that the Lift stopped at (in the order visited!)

### Example
Refer to the example test cases.
```cpp
queues = { {}, {0}, {}, {}, {2}, {3}, {} };
result = {0, 5, 4, 3, 2, 1, 0};
```
## Solution
The lift is only stop when `the lift is empty, and no people are waiting`.Thus, until that moment, the lift will repeat these actions: 
1. Go up: Because `The Lift always starts on the ground floor`, the first thing it does is going up.
2. Go down: Happens after the lift have reached the top floor.

For each $i$ floor:
- Remove the people inside the lift if their wanted floor is $i$th
- Add all the people whose wanted floor is in the same direction to the lift until the lift is full.
- If one of those action performed, store $i$ into result vector.
```cpp
#include <vector>

#define UP 1
#define DOWN 0

bool remove_people(std::vector<int> *lift, int floor){
    bool stop = false;
    for (auto person = lift->begin(); person != lift->end(); person++){
        if (*person == floor){
            lift->erase(person--);
            stop = true;
        }     
    }
    return stop;
}

bool add_people(std::vector<int> *lift, std::vector<int> *people, int floor, int capacity, int direction){
    bool stop = false;
    for(auto person = people->begin(); person != people->end(); person++){
        if((*person > floor && direction == UP) || (*person < floor && direction == DOWN)){
            stop = true;
            if(int(lift->size()) < capacity){
                lift->push_back(*person);
                people->erase(person--);
            }
        }
    }
    return stop;
}

std::vector<int> the_lift(std::vector<std::vector<int>> queues, int capacity) {
    std::vector<int> res = {0}, lift;
    int top = queues.size();
    bool unfinish = true;
    while(unfinish){
        unfinish = false;
        for(int floor = 0; floor < top; floor++){
            bool stop = remove_people(&lift, floor);
            stop = add_people(&lift, &queues[floor], floor, capacity, UP) || stop;
            if(!queues[floor].empty())
                unfinish = true;
            if(stop && res.back() != floor) res.push_back(floor);
        }
        for(int floor = top-1; floor >= 0; floor--){
            bool stop = remove_people(&lift, floor);
            stop = add_people(&lift, &queues[floor], floor, capacity, DOWN) || stop;
            if(!queues[floor].empty())
                unfinish = true;
            if(stop && res.back() != floor) res.push_back(floor);
        }
    }
    if(res.back()) res.push_back(0);
    return res;
}
```
