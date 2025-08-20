#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure for a patient node
typedef struct Patient {
    int id;
    char name[50];
    int age;
    char disease[50];
    struct Patient* next;
} Patient;

// Function to create a new patient node
Patient* createPatient(int id, char* name, int age, char* disease) {
    Patient* newPatient = (Patient*)malloc(sizeof(Patient));
    newPatient->id = id;
    strcpy(newPatient->name, name);
    newPatient->age = age;
    strcpy(newPatient->disease, disease);
    newPatient->next = NULL;
    return newPatient;
}

// Add patient to the end of the list
void addPatient(Patient** head, int id, char* name, int age, char* disease) {
    Patient* newPatient = createPatient(id, name, age, disease);
    if (*head == NULL) {
        *head = newPatient;
    } else {
        Patient* temp = *head;
        while (temp->next != NULL)
            temp = temp->next;
        temp->next = newPatient;
    }
    printf("Patient added successfully.\n");
}

// Display all patients
void displayPatients(Patient* head) {
    if (head == NULL) {
        printf("No patients to display.\n");
        return;
    }
    Patient* temp = head;
    printf("\nPatient List:\n");
    printf("ID\tName\t\tAge\tDisease\n");
    printf("-------------------------------------------\n");
    while (temp != NULL) {
        printf("%d\t%s\t\t%d\t%s\n", temp->id, temp->name, temp->age, temp->disease);
        temp = temp->next;
    }
}

// Delete patient by ID
void deletePatient(Patient** head, int id) {
    if (*head == NULL) {
        printf("List is empty.\n");
        return;
    }
    Patient* temp = *head;
    Patient* prev = NULL;

    // If head node holds the patient to be deleted
    if (temp != NULL && temp->id == id) {
        *head = temp->next;
        free(temp);
        printf("Patient with ID %d deleted.\n", id);
        return;
    }

    // Search for the patient
    while (temp != NULL && temp->id != id) {
        prev = temp;
        temp = temp->next;
    }

    // If patient not found
    if (temp == NULL) {
        printf("Patient with ID %d not found.\n", id);
        return;
    }

    // Unlink and delete node
    prev->next = temp->next;
    free(temp);
    printf("Patient with ID %d deleted.\n", id);
}

int main() {
    Patient* head = NULL;
    int choice, id, age;
    char name[50], disease[50];

    while (1) {
        printf("\nHospital Patient Management\n");
        printf("1. Add Patient\n");
        printf("2. Display Patients\n");
        printf("3. Delete Patient by ID\n");
        printf("4. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                printf("Enter Patient ID: ");
                scanf("%d", &id);
                printf("Enter Name: ");
                scanf(" %[^\n]%*c", name);  // to read string with spaces
                printf("Enter Age: ");
                scanf("%d", &age);
                printf("Enter Disease: ");
                scanf(" %[^\n]%*c", disease);
                addPatient(&head, id, name, age, disease);
                break;

            case 2:
                displayPatients(head);
                break;

            case 3:
                printf("Enter Patient ID to delete: ");
                scanf("%d", &id);
                deletePatient(&head, id);
                break;

            case 4:
                printf("Exiting program.\n");
                // Free memory before exit
                while (head != NULL) {
                    Patient* temp = head;
                    head = head->next;
                    free(temp);
                }
                return 0;

            default:
                printf("Invalid choice, please try again.\n");
        }
    }
    return 0;
}
