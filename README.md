import json
import os

# File to store contacts
CONTACTS_FILE = "contacts.json"

# Load contacts from file
def load_contacts():
    if os.path.exists(CONTACTS_FILE):
        with open(CONTACTS_FILE, "r") as file:
            return json.load(file)
    return []

# Save contacts to file
def save_contacts(contacts):
    with open(CONTACTS_FILE, "w") as file:
        json.dump(contacts, file, indent=4)

# Add a new contact
def add_contact(contacts):
    name = input("Enter Name: ").strip()
    phone = input("Enter Phone Number: ").strip()
    email = input("Enter Email: ").strip()
    address = input("Enter Address: ").strip()
    
    contact = {
        "name": name,
        "phone": phone,
        "email": email,
        "address": address
    }
    
    contacts.append(contact)
    save_contacts(contacts)
    print(f"Contact '{name}' added successfully!\n")

# View all contacts
def view_contacts(contacts):
    if not contacts:
        print("No contacts found.\n")
        return
    print(f"{'Name':<20} {'Phone':<15} {'Email':<25} {'Address':<30}")
    print("-"*90)
    for c in contacts:
        print(f"{c['name']:<20} {c['phone']:<15} {c['email']:<25} {c['address']:<30}")
    print()

# Search contact by name or phone
def search_contact(contacts):
    query = input("Enter name or phone to search: ").strip().lower()
    results = [c for c in contacts if query in c['name'].lower() or query in c['phone']]
    if not results:
        print("No contact found.\n")
        return
    print("Search Results:")
    for c in results:
        print(f"{c['name']} | {c['phone']} | {c['email']} | {c['address']}")
    print()

# Update a contact
def update_contact(contacts):
    name = input("Enter the name of the contact to update: ").strip().lower()
    for c in contacts:
        if c['name'].lower() == name:
            print("Leave blank to keep current value.")
            new_name = input(f"Name ({c['name']}): ").strip()
            new_phone = input(f"Phone ({c['phone']}): ").strip()
            new_email = input(f"Email ({c['email']}): ").strip()
            new_address = input(f"Address ({c['address']}): ").strip()
            
            if new_name: c['name'] = new_name
            if new_phone: c['phone'] = new_phone
            if new_email: c['email'] = new_email
            if new_address: c['address'] = new_address
            
            save_contacts(contacts)
            print("Contact updated successfully!\n")
            return
    print("Contact not found.\n")

# Delete a contact
def delete_contact(contacts):
    name = input("Enter the name of the contact to delete: ").strip().lower()
    for i, c in enumerate(contacts):
        if c['name'].lower() == name:
            confirm = input(f"Are you sure you want to delete {c['name']}? (y/n): ").strip().lower()
            if confirm == 'y':
                contacts.pop(i)
                save_contacts(contacts)
                print("Contact deleted successfully!\n")
            else:
                print("Deletion cancelled.\n")
            return
    print("Contact not found.\n")

# Main menu
def main():
    contacts = load_contacts()
    while True:
        print("=== Contact Management System ===")
        print("1. Add Contact")
        print("2. View Contacts")
        print("3. Search Contact")
        print("4. Update Contact")
        print("5. Delete Contact")
        print("6. Exit")
        choice = input("Enter your choice: ").strip()
        
        if choice == '1':
            add_contact(contacts)
        elif choice == '2':
            view_contacts(contacts)
        elif choice == '3':
            search_contact(contacts)
        elif choice == '4':
            update_contact(contacts)
        elif choice == '5':
            delete_contact(contacts)
        elif choice == '6':
            print("Exiting program. Goodbye!")
            break
        else:
            print("Invalid choice. Try again.\n")

if __name__ == "__main__":
    main()
