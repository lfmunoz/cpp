
# cpp



# Q1.1: The following Node struct is an entry in a singly-linked list. With this knowledge, implement the below functions.

```cpp
    struct Node
    {
        int value;
        Node* next;
    }

    // Reverses the order of the linked list such that the first node becomes the last, and the last node becomes the first.
    void ReverseList(Node** linkedList)
    {
        // Your code goes here.
        if(linkedList == nullptr) return;
        Node* tmp = nullptr;
        Node* previous = nullptr;

        while((*linkedList)->next != nullptr) {
            tmp = (*linkedList)->next;
            (*linkedList)->next = previous;
            previous = *linkedList;
            *linkedList = tmp;
        }
        (*linkedList)->next = previous;
    }

    // Removes all Nodes from the linked list with an odd number stored in their "value" member variable.
    void RemoveOdds(Node** linkedList)
    {
        // Your code goes here.
        if((*linkedList) == nullptr) return;
        if((*linkedList)->data % 2 == 1) {
            delete *linkedList;
            *linkedList = nullptr;
            return;
        }
        Node* previous = *linkedList; 
        Node* current = (*linkedList)->next;
        while(current != nullptr) {
            if(current->data % 2 == 1) {
                previous->next = current->next;
                delete current;
                current = previous->next;
            } else {
                previous = current;
                current = current->next;
            }
        }
    }
```



## Q1.2: Make a class, LinkedList, that has methods with equivalent behavior to the above functions. The class should be responsible for the memory management associated with all nodes. Do not erase the code from Q1, since I'd like to compare your implementations of the free functions to those of the member functions in this part.



```cpp
class SingleLinkedList
{
private:
    Node *root;
    int numOfNodes;

public:
    SingleLinkedList()
    {
        root = nullptr;
        numOfNodes = 0;
    }

    // return inserted Node
    Node* insert_back(int data)
    {
        // create new node to insert
        Node *newNode = new Node();
        newNode->data = data;
        // special case: when root node is nullptr
        if(root == nullptr) {
            root = newNode;
            return newNode;
        }

        Node *current = root;
        while(current->next != nullptr) {
            current = current->next;
        }
        current->next = newNode;

        return newNode;
    }

    void reverse() {
        if(root == nullptr) return;
        Node* tmp = nullptr;
        Node* previous = nullptr;

        while((root)->next != nullptr) {
            tmp = (root)->next;
            (root)->next = previous;
            previous = root;
            root = tmp;
        }
        (root)->next = previous;
    }

    void remove_odds() {
        if((root) == nullptr) return;
        Node* previous = root;
        Node* current = (root)->next;

        if((root)->data % 2 == 1 && root->next == nullptr) {
            delete root;
            root = nullptr;
            return;
        }
        if(root->data % 2 == 1) {
            delete root;
            root = current;
            previous = current;
            current = current->next;
        }

        while(current != nullptr) {
            if(current->data % 2 == 1) {
                previous->next = current->next;
                delete current;
                current = previous->next;
            } else {
                previous = current;
                current = current->next;
            }
        }
    }
};


```




## Q1.3: Add to LinkedList the following member functions:

    
```cpp
    // Returns the value stored in the node at the specified index.
    int GetValueAtIndex(int index) const {
        Node *current = root;
        int currentIdx = 0;
        while(current != nullptr) {
            if(currentIdx == index) return current->data;
            current = current->next;
            currentIdx += 1;
        }
        return -1; // is -1 a possible value?
    }
    
    // Creates a new entry at the back of the list with the specified value stored in it. Returns a pointer to the new node.
    Node* PushBackValue(int data) {
        // create new node to insert
        Node *newNode = new Node();
        newNode->data = data;
        // special case: when root node is nullptr
        if(root == nullptr) {
            root = newNode;
            return newNode;
        }

        Node *current = root;
        while(current->next != nullptr) {
            current = current->next;
        }
        current->next = newNode;

        return newNode;
    }
    
    // Creates a new entry at the specified index of the list with the specified value stored in it. Returns a pointer to the new node.
    Node* InsertValue(int value, int index) {
        // special case: we insert at front
        if(index ==0) {
            return insert_front(value);
        }

        // create new node to insert
        Node *newNode = new Node();
        newNode->data = data;

        Node *current = root;
        int currentIdx = 1;
        while(current->next != nullptr) {
            if(currentIdx == index) {
                newNode->next = current->next;
                current->next = newNode;
                return newNode;
            }
            current = current->next;
            currentIdx += 1;
        }
        // if index not found will just insert that last position
        current = newNode;
        return newNode;
    }
    
    // Removes the specified node from the linked list.
    void RemoveNode(Node* node) {
        Node *current = root;
        if(current == node) {
            root = current->next;
            delete current;
            numOfNodes -= 1;
            return;
        }
        while(current->next != nullptr && current->next != node) {
            current = current->next;
        }

        // at this point next is our node or is a nullptr
        if(current->next == nullptr) {
            return;
        }

        Node *next = current->next->next;
        delete current->next;
        current->next = next;
    }
    
    // Removes the node containing the first instance of the specified value from the linked list.
    void RemoveValue(int data) {
        Node *current = root;
        // root node is nullptr
        if(current == nullptr) return;
        // root node has our data
        if(current->data == data) {
            root = current->next;
            delete current;
            numOfNodes -= 1;
            return;
        }
        // check that next item has our data and that it is not a nullptr
        while(current->next != nullptr && current->next->data != data) {
            current = current->next;
        }
        // at this point next has our data or is a nullptr
        if(current->next == nullptr) {
            return;
        }

        Node *next = current->next->next;
        delete current->next;
        current->next = next;
    }
```
    
## Q1.4: Refactor make LinkedList into an abstract class with two subclasses: SinglyLinkedList and DoublyLinkedList.

```cpp

class Node
{
public:
    int data = 0;
    Node *next = nullptr;
    Node *prev = nullptr;
};

class LinkListInterface {
private:
    Node *root;
public:
    // pure virtual function
    virtual Node* insert_back() = 0;
    virtual Node* print() = 0;
      
};


class SinglyLinkedList: LinkListInterface {
public:
    virtual Node* insert_back() {

    }
};

class DoublyLinkedList : LinkListInterface{
    virtual Node* insert_back() {

    }
};

```

## Q1.5: Make SinglyLinkedList and DoublyLinkedList thread-safe.

Easiest to put a mutex on the interface that is used when entering and exiting a function but might be more performant
if the node itself had a mutex.

```cpp
class LinkListInterface {
private:
    Node *root;
    std::mutex mux;
public:
    // pure virtual function
    virtual Node* insert_back() = 0;
    virtual Node* print() = 0;
      
};


class SinglyLinkedList: LinkListInterface {
public:
    virtual Node* insert_back() {
        std::lock_guard<std::mutex> lock(mux);
         // ... code
    }
};

class DoublyLinkedList : LinkListInterface{
    virtual Node* insert_back() {
        std::lock_guard<std::mutex> lock(mux);
        // ... code
    }
};
```


# Q2: Declare a free function in a cpp file and use it in another cpp file.


```cpp

// test.cpp
int freeFunction() {

}

```

```cpp

#include 'test.cpp'

freeFunction();

```



# Q3: You have a class Character with a const member function CalculateDistanceWalked() that uses some data from breadcrumbs to determine how far the character has walked since they were created. To save on performance, the team has decided they want to cache the distance walked in a member variable and only run the calculation if more than 2 seconds have passed since CalculateDistanceWalked last ran its calculation. How would you implement this? Remember, CalculateDistnaceWalked is a const function.

```cpp
uint64_t timeSinceEpochMillisec() {
  using namespace std::chrono;
  return duration_cast<milliseconds>(system_clock::now().time_since_epoch()).count();
}


class Character {

private:
    int distanceWalkedCache = 0;
    uint64_t timeCacheUpdated = 0;

public:

    int CalculateDistanceWalked() const {

    }

    void moveCharacter(int x, int y) {
        uint64_t currentTime = timeSinceEpochMillisec()
        if(currentTime > timeSinceCacheUpdate + 2000) {
            distanceWalkedCache = CalculateDistanceWalked()
            timeCacheUpdated = currentTime;
        }
        ...
    }

}
```




# Q4: Write a function, Max, that takes two values and returns the greater of the two. Max should be able to take any type and should be evaluated at compile time.

```c
template<typename T>
T Max(const T& a, const T& b) {
    if ( a > b) return a
    else return b;
} 
```

Not sure on this, maybe I need to add `constexpr` or `inlnie`. Think template is evaluted at compile time where it makes a copy with a specific type
but not sure when the compiler will evaluate entire expression and put a number instead of a function inline. 

# Q5: When would Max not be able to evaluate at compile-time?

If the arguments are not known at compile time then Max cannot be evaluated at compile time.


# Q6: Create a struct with at least five member variables (the member variables can be anything you choose). Initialize the values to something sensible.

```cpp

struct User {
    char username[32];
    char pasword[32];
    long lastLogin;
    unsigned int age; 
    int popularity;
};

struct User john; 
john.username = "john";
john.password= "johnpassword";
john.lastlogin =  0L;
john.age = 30;
john.popularity = 1;
```


# Q7: What is the size of this struct? Why?

32 bytes + 32 bytes + 8 bytes (long) + 4 bytes (int) + 4 bytes (int) = 80 bytes


# Q8: What are the different types of casts? When would you use them? Provide examples.

**Downcast primitives** when you lose bits on a number.

```cpp
int x = 100;
short y = x; 
```
Here short depending on platform is 2 bytes but int is 4 bytes so you lose the ability to represnt the top significant bits.

**Upcast primitives** when you put place a number with less bits into a variable that can how more bits, there is no problem with this.

```cpp
short x = 100;
int y = x; 
```

**casting objects** when you have a relationship between objects you can treat them as if they are the subclass object because it has less features.

```cpp

class Shape;

class Square : public Shape;


Square square = Square();
Shape* shape = square;
```



# Q9: In an online game, the player can toggle the visibility of each of their pieces equipment on or off (pieces of equipment may include helmet, weapon, body armor, boots, gloves, etc.). This data is cached on the client, but stored on a server so the player preferences can be loaded in subsequent play sessions on other devices. How would you store this data on the client? How would you store it in a message sent over the network?

I would have a Set of equipmentIds assigned to player and a global lookup table to get actual equipment.

```cpp

map<Long, Equipment> equipmentCache;

class PlayerInvetory {
    // equipmentId -> isVissble
    map<Long, boolean> equipment;
};

```

PlayerInvetory should be serializable, i.e, can be turned to JSON and sent over network and also saved to disk.

```json
{
    PlayerInvetory: {
        equipment: {
            45: false,
            58: true,
            123: false,
            2344: true
        }
    }
}
```


# Q10: The exit button on the main menu of a game has different behavior depending on where the game is running:
    A) If the game is running in a development environment, it opens a dialog displaying some information about the game's performance during the developer's play session.
    B) If the game is running in a retail environment, it sends the performance data across the network via a telemetry service.
How would you handle this exit button having these two different behaviors?


I would just use an if statement that takes different code paths. Best to do simplest thing possible because think maintainability and readability
is generally the highest priority.

```cpp
void OnExitButtonClick() {
    if(ENVIRONMENT == ENV.DEVELOPMENT) {
        displayGamePerformanceInformation();
    } else {
        sendGamePerformance();
    }
}

```



