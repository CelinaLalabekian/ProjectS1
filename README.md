#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

struct patient {
    int id;
    char patientName[50];
    char patientAddress[50];
    char disease[50];
    char date[12];
    int age;  // Added age field for analysis
};

struct doctor {
    int id;
    char name[50];
    char address[50];
    char specialize[50];
    char date[12];
};

struct patient patients[100];  // Assuming a maximum of 100 patients
int patientCount = 0;          // Keep track of the number of patients

FILE *fp;

// Function declarations
void admitPatient();
void integrateData();
void basicDataAnalysis();

int main() {
    int ch;

    while (1) {
        system("cls");
        printf("<== Hospital Management System ==>\n");
        printf("1. Admit Patient\n");
        printf("2. Integrate Data\n");
        printf("3. Basic Data Analysis\n");
        printf("0. Exit\n\n");
        printf("Enter your choice: ");
        scanf("%d", &ch);

        switch (ch) {
            case 0:
                exit(0);

            case 1:
                admitPatient();
                break;

            case 2:
                integrateData();
                break;

            case 3:
                basicDataAnalysis();
                break;

            default:
                printf("Invalid Choice...\n\n");
        }
        printf("\n\nPress Any Key To Continue...");
        getchar(); // Use getchar instead of getch
    }

    return 0;
}

void admitPatient() {
    char myDate[12];
    time_t t = time(NULL);
    struct tm tm = *localtime(&t);
    sprintf(myDate, "%02d/%02d/%d", tm.tm_mday, tm.tm_mon + 1, tm.tm_year + 1900);
    strcpy(patients[patientCount].date, myDate);

    // Assume age is collected during admission
    printf("Enter Patient Age: ");
    if (scanf("%d", &patients[patientCount].age) != 1) {
        printf("Invalid input for age. Please enter a valid integer.\n");
        return;
    }

    fp = fopen("patient.txt", "ab");
    if (fp == NULL) {
        printf("Error opening patient.txt for writing.\n");
        return;
    }

    printf("Enter Patient id: ");
    if (scanf("%d", &patients[patientCount].id) != 1) {
        printf("Invalid input for patient ID. Please enter a valid integer.\n");
        fclose(fp);
        return;
    }

    printf("Enter Patient name: ");
    getchar(); // Consume the newline character left in the buffer
    fgets(patients[patientCount].patientName, sizeof(patients[patientCount].patientName), stdin);
    patients[patientCount].patientName[strcspn(patients[patientCount].patientName, "\n")] = '\0';

    printf("Enter Patient Address: ");
    fgets(patients[patientCount].patientAddress, sizeof(patients[patientCount].patientAddress), stdin);
    patients[patientCount].patientAddress[strcspn(patients[patientCount].patientAddress, "\n")] = '\0';

    printf("Enter Patient Disease: ");
    fgets(patients[patientCount].disease, sizeof(patients[patientCount].disease), stdin);
    patients[patientCount].disease[strcspn(patients[patientCount].disease, "\n")] = '\0';

    printf("\nPatient Added Successfully\n");

    fwrite(&patients[patientCount], sizeof(struct patient), 1, fp);
    fclose(fp);

    patientCount++;
}

void integrateData() {
    printf("Data Integration Placeholder\n");
    // You can perform integration logic here based on your requirements
}

void basicDataAnalysis() {
    int totalAge = 0;

    for (int i = 0; i < patientCount; i++) {
        totalAge += patients[i].age;
    }

    double averageAge = patientCount > 0 ? (double)totalAge / patientCount : 0;

    printf("Basic Data Analysis:\n");
    printf("Average Patient Age: %.2lf\n", averageAge);
}

