# Operating Systems Assignment
# Starve-Free Readers-Writers Problem
#### KHUSHI 18115051
#### Code + Documentation for Starve free Readers-Writers Problem

The Readers-Writers Problem is well-known in computer science. A resource can be accessed by readers, who do not modify the resource, and writers, who can modify the resource. When a writer is modifying the resource, no-one else (reader or writer) can access it at the same time, else another writer could corrupt the resource, and another reader could read a partially modified (inconsistent) value. There are three variations possible for the same: which are defined as first, second and third readers-writers problem respectively. The first gives readers the priority and can result in starvation for writers and the second one gives priority to writers and can lead to starvation for readers. The third Readers-Writers Problem deals with giving a Starve-free environment for both readers and writers. The proposed solution for fair starve-free access for readers and writers both is as discussed below.

## Fundamental Concept:
Give neither priority: all readers and writers will be granted access to the resource in their order of arrival. If a writer arrives while readers are accessing the resource, it will wait until those readers free the resource, and then modify it. New readers arriving in the meantime will have to wait.

## Semaphore:
The solution thus proposed, uses Semaphores following a First-In-First-Out strategy to provide access control of the resource to either readers or writers.

### Code for defining Semaphore with FIFO strategy:

```cpp
//Semaphore following FIFO strategy.
struct Semaphore
{
  int value = 1;
  FIFO_Queue* Q = new FIFO_Queue();
}
    
void wait(Semaphore *S,int* process_id)
{
  S->value--;
  if(S->value < 0)
  {
  S->Q->push(process_id);
  block(); //This function will block the proccess until it's woken up.
           //The process will remain in the waiting queue till it is waken up by the wakeup() system calls
           //This is a type of non-busy waiting
  }
}
    
void signal(Semaphore *S)
{
  S->value++;
  if(S->value <= 0){
  int* PID = S->Q->pop();
  wakeup(PID); //this function will wakeup the process with the given pid using system calls
  }
}


//Queue which will allow us to form a FIFO semaphore
struct FIFO_Queue
{
    ProcessBlock* front, rear;
    
    int* pop()
    {
        if(front == NULL){
            return -1;            // Error : underflow.
        }
        else{
            int* val = front->value;
            front = front->next;
            if(front == NULL)
            {
                rear = NULL;
            }
            return val;
        }
    }
    
    void* push(int* val)
    {
        ProcessBlock* blk = new ProcessBlock();
        blk->value = val;
        if(rear == NULL)
        {
            front = rear = n;
            
        }
        else
        {
            rear->next = blk;
            rear = blk;
        }
    }
    
}

// A Process Block.
struct ProcessBlock{
    ProcessBlock* next;
    int* process_block;
}
```
A FIFO semaphore is thus implemented. A queue is used to manage the waiting processes. The process gets blocked after pushing itself onto the queue and is woken up in FIFO order when some other process releases the semaphore.

### Initialization:
The solution uses 3 semaphores. The <b>turn</b> semaphore is used to specify whose chance is to next enter the critical section. The process with this semaphore gets the next chance to enter the critical section. The <b>rwt</b> semaphore is required to access the critical section. <b>rmutex</b> semaphore is required to change the readcount variable which maintain the number of active readers.
```cpp
int read_count = 0;                      //Variable representing the number of readers 
                                         //currently executing the critical section
Semaphore turn = new Semaphore();        //Semaphore representing the order in which the writers and 
                                         //readers are requesting access to critical section
Semaphore rwt = new Semaphore();         //Semaphore required to access the critical section
Semaphore r_mutex = new Semaphore();     //Semaphore required to change the read_count variable
```


