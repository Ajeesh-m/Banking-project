class Bank:
    def __init__(self):
        self.accounts = {}
        self.accounts['10001'] = {'username': 'John', 'password': 'john123', 'balance': 6000}
        self.accounts['10002'] = {'username': 'Rahul', 'password': 'rahul123', 'balance': 250}
        self.accounts['10003'] = {'username': 'Sam', 'password': 'sam123', 'balance': 5000}

    def create_account(self, username, account_number, password):
        if account_number not in self.accounts:
            self.accounts[account_number] = {'username': username, 'password': password, 'balance': 0}
            print("Account created successfully!")
        else:
            print("Account already exists. Please choose a different account number.")

    def login_required(func):
        def wrapper(self, account_number, password, *args, **kwargs):
            if account_number in self.accounts and self.accounts[account_number]['password'] == password:
                return func(self, account_number, password, *args, **kwargs)
            else:
                return "Invalid credentials. Please log in first."
        return wrapper

    @login_required
    def deposit(self, account_number, password, amount):
        self.accounts[account_number]['balance'] += amount
        return self.accounts[account_number]['balance']

    @login_required
    def withdraw(self, account_number, password, amount):
        if amount > self.accounts[account_number]['balance']:
            return "Insufficient funds"
        else:
            self.accounts[account_number]['balance'] -= amount
            return self.accounts[account_number]['balance']

    @login_required
    def check_balance(self, account_number, password):
        return self.accounts[account_number]['balance']

def main():
    bank = Bank()
    while True:
        print("\n1. Create Account\n2. Deposit\n3. Withdraw\n4. Check Balance\n5. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            username = input("Enter your username: ")
            account_number = input("Enter desired account number: ")
            password = input("Enter desired password: ")
            bank.create_account(username, account_number, password)
        elif choice in ['2', '3','4']:
            account_number = input("Enter account number: ")
            password = input("Enter your password: ")

            if choice == '2':
                amount = float(input("Enter the amount to deposit: "))
                print("Balance after deposit:", bank.deposit(account_number, password, amount))
            elif choice == '3':
                amount = float(input("Enter the amount to withdraw: "))
                print("Balance after withdrawal:", bank.withdraw(account_number, password, amount))
            elif choice == '4':
                print("Current Balance:", bank.check_balance(account_number, password))
        elif choice == '5':
            print("Thank you for using our banking system.\nExiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
