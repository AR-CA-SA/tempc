
// Exam2_Studentlist.c
/***************************************************************************************
 This assignment is based on previous assignment for the following functinalities:

    This program will be able to maintain a list of students in enrooled in a class offered by DTCC for a semester
    Complete this program to demonstrate your understanding how to use dymamically allocated 
    momeries at needed bases to add unlimited students to one class. 

    With this program, the user can add new a new student to the linked list, delete a student from the list
    and view all the students enrolled in this class. 

    The linked list of one way linked structure which has data elements for the basic information for a student. 

The linked list created with previous assignment can only exist in computer memory and will be lost when user exit 
the program.
With this assignment, you will use the file input and ouput APIs to save the student information to a file on 
the hardware disk, then user can have the the option to read the saved students back to memory to edit the list
as adding new student, delete a student, and resave it to the file. 
**********************************************************************************************/

/**********************************************************************************************
 * Please include your name, course name, date  at the beginning of your code
 * ********************************************************************************************/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Define the structure for a student

typedef struct Student {
    char name[50];
    int id;
    int age;
    char major[50];
    float grade;
    struct Student* next;
} Student;

// Function to create a new student node
Student* create_student(char* name, int id, int age, char* major, float grade) {
    Student* new_student = (Student*)malloc(sizeof(Student));
    strcpy(new_student->name, name);
    new_student->id = id;
    new_student->age = age;
    strcpy(new_student->major, major);
    new_student->grade = grade;
    new_student->next = NULL;
    return new_student;
}

/*************************************************************************************
 Function to add a student to the linked list
 in this function you should create a new student with create_student function and then 
 add this new student to the linked list

***************************************************************************************/
void add_student(Student** head, char* name, int id, int age, char* major, float grade) {



    Student* new_student = create_student(name, id , age , major, grade);
    if (*head == NULL) {
        *head = new_student;
    }
    
    Student* curr = *head;
    while(curr->next != NULL){
        curr = curr->next; 
    }

    curr->next = new_student;

    new_student->next = NULL;

}

/***********************************************************************************************
 * This function will accept the pointer to the student linkded list and a student Id, then search
 * the student linked list to find the matched student and remove the student from the list. after remove the 
 * matched student from the list, you need keep all other students in the list and free the momeries used 
 * by the removed student
 *******************************************************************************************/
void delete_student(Student** head, int id) {
    if(*head == NULL){
        exit(1);
    }

    if((*head)->id ==id){
        Student* to_remove = *head;
        *head = (*head)->next;
        free(to_remove);
        return;
    }  

    Student* temp = *head;

        while(temp->next != NULL){
 
        if(temp->next->id == id){

            Student* to_remove = temp->next;
            temp->next = temp->next->next;
            free(to_remove);
            return;

        }
        temp = temp->next;      
    }

    if(temp->id==id){
        free(temp);
        temp = NULL;
    }
   
}


void print_students(Student* head) {
    Student* current = head;

    
    while (current != NULL) {
        printf("NAME: %s | ID : %d | AGE: %d | MAJOR : %s | GRADE : %.2f | \n",
        current->name, current->id, current->age, current->major, current->grade );
        current = current->next;
   
    }
    printf("\n");

}
/**************************************************************************************************
 Function to save the student list to a file
 This function will accept the student linked list pointer and a file name. 
 You need to complete the code in this function to save all the students in the linked list to a file.

***************************************************************************************************/ 
char* contents = NULL;

void save_students_to_file(Student* head, const char* filename) {
    FILE* file = fopen(filename, "w");
    if (file == NULL) {
        printf("Error opening file for writing\\n");
        return;
    }
    contents = malloc(100 * size_of(__CHAR32_TYPE__));
 // start your code here
    while (current != NULL) {
        contents[current->name, current->id, current->age, current->major, current->grade]; 
        current = current->next;
        
    }
    

    fclose(file);
    printf("Student list saved to %s\\n", filename);
}

/***************************************************************************************************
 Function to read the student list from a file

 This function will accept a student linked list pointer and a file name.
 You need to complete the code in this function so that the program can read all the students' information
 from a file, and rebuild the linked list in the memory so user can edited the list from the file for adding 
 new students, delete students, viewing the list and save the updated list back to a file. 
 You have to rebuild the linked list, not just print out the student info to the console. 
******************************************************************************************************/
// 
void read_students_from_file(Student** head, const char* filename) {
    FILE* file = fopen(filename, "r");
    if (file == NULL) {
        printf("Error opening file for reading\\n");
        return;
    }
 // start your code here: 
    
    fclose(file);
    printf("Student list read from %s\\n", filename);
}

/**********************************************************************************
 * This function will free up all the memories used by the all linked list befor
 * exit the program. 
***********************************************************************************/
void free_mem(Student *Head)
{

	struct Student  *ptrthis, *ptrfree;

	if (Head == NULL)
		exit(0);

	ptrthis = Head;
	do
	{
		ptrfree = ptrthis;
		ptrthis = ptrthis->next;
		free(ptrfree);
	} while (ptrthis != NULL);

	exit(0);
}



/******************************************************************************************************
 * the main function have the starting code with a menu so users can have choices to
 * 1) add a new student
 * 2) delete a student
 * 3) show all the students enrolled in the class
 * 4) exit the program. 
 * 
 * You can keep the starting code as it is, or you can implement your code to replace them, but you have to 
 * provide the interaction to users for doing above listed functions. 
**********************************************************************************************************/
int main() {
    Student* head = NULL;
    int choice;
    char name[50];
    int id;
    int age;
    char major[50];
    float grade;
    char c;
    char filename[50];

    while (1) {
        printf("1. Add student\n");
        printf("2. Delete student\n");
        printf("3. List students\n");
        printf("4. Save students to file\\n");
        printf("5. Read students from file\\n");
        printf("6. Exit\\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        scanf("%c", &c); // get rid off the ener for next read. 


        switch (choice) {
            case 1:
                printf("Enter name: ");
                scanf("%[^\n]c", name);
                printf("Enter ID: ");
                scanf("%d", &id);
                printf("Enter age: ");
                scanf("%d", &age);
                scanf("%c", &c);  // get rid off the enter for next read
                printf("Enter major: ");
                scanf("%[^\n]c", major);
                printf("Enter grade: ");
                scanf("%f", &grade);
                add_student(&head, name, id, age, major, grade);
                break;
            case 2:
                printf("Enter ID of student to delete: ");
                scanf("%d", &id);
                delete_student(&head, id);
                break;
            case 3:
                print_students(head);
                break;
            case 4:
                printf("Enter filename to save to: ");
                scanf("%s", filename);
                save_students_to_file(head, filename);
                break;
            case 5:
                printf("Enter filename to read from: ");
                scanf("%s", filename);
                read_students_from_file(&head, filename);
                break;
            case 6:
                free_mem(head);
            default:
                printf("Invalid choice!\\n");
        }
    }

    return 0;
}
