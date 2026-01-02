
import java.util.Scanner;

class Member {
    String name;
    String address;  
    String contact;
    String certification;
    int yearsOfExperience;

    // Constructor ( initialize)
    Member(String memberName, String memberAddress, String memberContact, String memberCertification, int memberYearsOfExperience) {
        name = memberName;
        address = memberAddress;
        contact = memberContact;
        certification = memberCertification;
        yearsOfExperience = memberYearsOfExperience; 
    }

    void getMenuChoice() {
        System.out.println(name + " | " + address + " | " + contact + " | " + certification + " | " + yearsOfExperience + " years of experience");
    }
}

public class DOEA {
    static Member[] members = new Member[10];
    static int totalMembers = 0;

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        while (true) {
            showMenu();

            System.out.print("Choose: ");
            int choice = input.nextInt();
            input.nextLine(); 

            if (choice == 1) {
                addMember(input);
            } else if (choice == 2) {
                searchMemberByName(input);
            } else if (choice == 3) {
                editMember(input);
            } else if (choice == 4) {
                displayAllMembers();
            } else if (choice == 5) {
                computeAverageExperience();
            } else if (choice == 6) {
                System.out.println("Thank you! Program ended.");
                break;
            } else {
                System.out.println("Invalid choice. Try again.");
            }
        }

        input.close();
    }

    // MENU
    static void showMenu() {
        System.out.println("\n===== Menu of Transactions =====");
        System.out.println("1. Add Member");
        System.out.println("2. Search Member by Name");
        System.out.println("3. Edit Member Information");
        System.out.println("4. Display All Members");
        System.out.println("5. Compute Average Years of Experience");
        System.out.println("6. Exit Program");
    }

// num 1. ADD MEMBER (experience replaced with address)
    static void addMember(Scanner input) {
        System.out.print("Enter full name (First , Last): ");
        String name = input.nextLine();

        if (!ValidateName(name)) {
            System.out.println("Error: Invalid name format");
            return;
        }

        System.out.print("Enter address: ");
        String address = input.nextLine();

        System.out.print("Enter contact number: ");
        String contact = input.nextLine();
        
        
 // bawal mag sugod sa lain numbers exactly ( 09 )
        if (!ValidateContactNumber(contact)) {
            System.out.println("Invalid contact number. Must start with 09 and have 11 digits.");
            return;
        }

        System.out.print("Enter certification level (Choices : Beginner, Skilled, Advanced): ");
        String cert = input.nextLine();
        
        
 // result if wala sa choices na gi provide ang gi pili.
        if (!ValidateCertificationLevel(cert)) {
            System.out.println("Invalid certification level.");
            return;
        }


        System.out.print("Enter years of experience: ");
        int yearsOfExperience = input.nextInt();
        input.nextLine();

// result if mahuman nag input ang user sa info 
        members[totalMembers++] = new Member(name, address, contact, cert, yearsOfExperience); 
        System.out.println("Member added successfully!");
    }
    // num.2 SEARCH MEMBER
    static void searchMemberByName(Scanner input) {
        System.out.print("Enter name to search: ");
        String name = input.nextLine();

        for (int i = 0; i < totalMembers; i++) {
            if (members[i].name.split(" ")[0].equalsIgnoreCase(name)) {
                System.out.println(" Member found");
               members[i].getMenuChoice();
                return;
            }
        }
        System.out.println("Member not found.");
    }

// num 3. EDIT MEMBER (user chooses what to update)
static void editMember(Scanner input) {
    System.out.print("Enter name to update: ");
    String name = input.nextLine();

    for (int i = 0; i < totalMembers; i++) {
        if (members[i].name.split(" ")[0].equalsIgnoreCase(name)) {

            while (true) {
                System.out.println("\nWhat do you want to update?");
                System.out.println("1. Address");
                System.out.println("2. Contact Number");
                System.out.println("3. Certification Level");
                System.out.println("4. Years of Experience");
                System.out.println("5. Exit Update Menu");

                System.out.print("Choose: ");
                int choice = input.nextInt();
                input.nextLine();

                switch (choice) {
                    case 1:
                        System.out.print("Enter new address: ");
                        members[i].address = input.nextLine();
                        System.out.println("Address updated!");
                        break;

                    case 2:
                        System.out.print("Enter new contact number: ");
                        String newContact = input.nextLine();
                        if (!ValidateContactNumber(newContact)) {
                            System.out.println("Invalid contact number.");
                        } else {
                            members[i].contact = newContact;
                            System.out.println("Contact number updated!");
                        }
                        break;

                    case 3:
                        System.out.print("Enter new certification (Beginner, Skilled, Advanced): ");
                        String newCert = input.nextLine();
                        if (!ValidateCertificationLevel(newCert)) {
                            System.out.println("Invalid certification level.");
                        } else {
                            members[i].certification = newCert;
                            System.out.println("Certification updated!");
                        }
                        break;

                    case 4:
                        System.out.print("Enter new years of experience: ");
                        members[i].yearsOfExperience = input.nextInt();
                        input.nextLine();
                        System.out.println("Year experience updated!");
                        break;

                    case 5:
                        System.out.println("Finished updating.");
                        return;

                    default:
                        System.out.println("Invalid choice. Try again.");
                }
            }
        }
    }
    System.out.println("Member not found.");
}


    // num 4. DISPLAY MEMBERS
    static void displayAllMembers() {
        if (totalMembers == 0) {
            System.out.println("No members available.");
            return;
        }

        for (int i = 0; i < totalMembers; i++) {
            members[i].getMenuChoice();
        }
    }

    //num 5. COMPUTE AVERAGE YEARS OF EXPERIENCE
    static void computeAverageExperience() {
        if (totalMembers == 0) {
            System.out.println("No members to compute average experience.");
            return;
        }

        int totalExperience = 0;
        for (int i = 0; i < totalMembers; i++) {
            totalExperience += members[i].yearsOfExperience;
        }
// pag compute sa avreage
        double averageExperience = totalExperience / (double) totalMembers;
        System.out.println("Average years of experience: " + averageExperience);
    }

    // VALIDATIONS

    // condition sa Name dapat Capital letter ang first letter
    static boolean ValidateName(String name) {
        if (name == null) return false;
        
        //REGULAR EXPRESSION
        return name.matches("^[A-Z][a-zA-Z]*(\\s+[A-Z][a-zA-Z]*)+$");
    }

    // condition sa Contact number dapat ( 09 ) mag start
    static boolean ValidateContactNumber(String contact) {
        
        //REGULAR EXPRESSION
        return contact.matches("^09\\d{9}$");
    }

    // condition sa Certification (logical ||)
    static boolean ValidateCertificationLevel(String cert) {
        return cert.equals("Beginner") ||
               cert.equals("Skilled") ||
               cert.equals("Advanced");
    }
}
