# SIMPLE-ATM-PIN-STIMULATOR
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define DEFAULT_PIN "1234"
#define MAX_PIN_ATTEMPTS 3

// Global variables
float balance = 1000.00;  // Initial balance
char pin[5] = DEFAULT_PIN;

// Function prototypes
void displayWelcomeScreen();
int verifyPIN();
void displayMainMenu();
void checkBalance();
void withdrawMoney();
void depositMoney();
void changePIN();
void clearScreen();
void waitForEnter();

int main() {
    int choice;

    displayWelcomeScreen();

    // Verify PIN before proceeding
    if (!verifyPIN()) {
        printf("\nToo many incorrect attempts. Card blocked.\n");
        return 1;
    }

    while (1) {
        displayMainMenu();
        printf("\nEnter your choice (1-5): ");
        if (scanf("%d", &choice) != 1) {
            printf("\nInvalid input! Please enter a number.\n");
            while (getchar() != '\n'); // Clear input buffer
            continue;
        }

        switch (choice) {
            case 1:
                checkBalance();
                break;
            case 2:
                withdrawMoney();
                break;
            case 3:
                depositMoney();
                break;
            case 4:
                changePIN();
                break;
            case 5:
                printf("\nThank you for using our ATM. Goodbye!\n");
                return 0;
            default:
                printf("\nInvalid choice! Please try again.\n");
        }

        waitForEnter();
        clearScreen();
    }

    return 0;
}

void displayWelcomeScreen() {
    clearScreen();
    printf("*************************************************\n");
    printf("*                 WELCOME TO ATM                *\n");
    printf("*************************************************\n");
}

int verifyPIN() {
    char input_pin[10];
    int attempts = 0;

    while (attempts < MAX_PIN_ATTEMPTS) {
        printf("\nPlease enter your 4-digit PIN: ");
        scanf(" %9s", input_pin);

        if (strlen(input_pin) != 4) {
            printf("\nPIN must be exactly 4 digits!\n");
            continue;
        }

        if (strcmp(input_pin, pin) == 0) {
            return 1; // PIN correct
        } else {
            attempts++;
            printf("\nIncorrect PIN! %d attempt(s) remaining.\n", MAX_PIN_ATTEMPTS - attempts);
        }
    }

    return 0; // PIN verification failed
}

void displayMainMenu() {
    printf("\n*************************************************\n");
    printf("*                 ATM MAIN MENU                 *\n");
    printf("*************************************************\n");
    printf("1. Check Balance\n");
    printf("2. Withdraw Money\n");
    printf("3. Deposit Money\n");
    printf("4. Change PIN\n");
    printf("5. Exit\n");
    printf("*************************************************\n");
}

void checkBalance() {
    printf("\nYour current balance is: $%.2f\n", balance);
}

void withdrawMoney() {
    float amount;

    printf("\nEnter amount to withdraw: $");
    if (scanf("%f", &amount) != 1) {
        printf("\nInvalid input! Please enter a valid number.\n");
        while (getchar() != '\n'); // Clear input buffer
        return;
    }

    if (amount <= 0) {
        printf("\nInvalid amount!\n");
    } else if (amount > balance) {
        printf("\nInsufficient funds!\n");
    } else {
        balance -= amount;
        printf("\nWithdrawal successful! Please take your cash.\n");
        printf("Remaining balance: $%.2f\n", balance);
    }
}

void depositMoney() {
    float amount;

    printf("\nEnter amount to deposit: $");
    if (scanf("%f", &amount) != 1) {
        printf("\nInvalid input! Please enter a valid number.\n");
        while (getchar() != '\n'); // Clear input buffer
        return;
    }

    if (amount <= 0) {
        printf("\nInvalid amount!\n");
    } else {
        balance += amount;
        printf("\nDeposit successful!\n");
        printf("New balance: $%.2f\n", balance);
    }
}

void changePIN() {
    char new_pin[10];
    char confirm_pin[10];

    printf("\nEnter new PIN (4 digits): ");
    scanf(" %9s", new_pin);

    if (strlen(new_pin) != 4) {
        printf("\nPIN must be exactly 4 digits!\n");
        return;
    }

    printf("Confirm new PIN: ");
    scanf(" %9s", confirm_pin);

    if (strcmp(new_pin, confirm_pin) == 0) {
        strcpy(pin, new_pin);
        printf("\nPIN changed successfully!\n");
    } else {
        printf("\nPINs do not match!\n");
    }
}

void clearScreen() {
#ifdef _WIN32
    system("cls");    // Windows
#else
    system("clear");  // Linux / Mac
#endif
}

void waitForEnter() {
    printf("\nPress Enter to continue...");
    while (getchar() != '\n'); // Clear leftover input
    getchar(); // Wait for Enter key
}
