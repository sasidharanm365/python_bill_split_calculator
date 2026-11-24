# Bill Split Calculator

A Python command-line application that helps groups divide shared expenses accurately and fairly. It supports equal bill splitting, custom contributions, and optional tip calculation.

## Features

- **Equal Split:** Divides the total bill equally among all participants.
- **Custom Split:** Accepts the amount paid by each participant and checks that the contributions match the final bill.
- **Tip Calculation:** Adds an optional tip based on a percentage.
- **Input Validation:** Checks bill amounts, participant counts, and numeric entries.
- **Exception Handling:** Handles invalid inputs without abruptly terminating the program.
- **Rupee Display:** Shows calculated amounts using the ₹ symbol.

## Technologies Used

- Python 3
- Python standard library

## Project Structure

```text
Bill-Split-Calculator/
├── main.py
├── README.md
├── requirements.txt
├── .gitignore
└── screenshots/
```

## How to Run

1. Install Python 3.8 or later.
2. Download or clone this repository.
3. Open a terminal in the project folder.
4. Run:

   ```bash
   python main.py
   ```

   If your system uses `python3`, run:

   ```bash
   python3 main.py
   ```

5. Enter the bill amount, optional tip, number of people, and their names.
6. Choose `1` for Equal Split or `2` for Custom Split.

## Example Output

For a ₹1000 bill shared equally by four people:

```text
Welcome to the Bill Split Calculator!
Enter the total bill amount (₹): 1000
Would you like to add a tip? (yes/no): no

Total amount to be split: ₹1000.00
Enter number of people: 4
Enter name of person 1: Arthur
Enter name of person 2: John
Enter name of person 3: Dutch
Enter name of person 4: Micah

Split Type:
1. Equal Split
2. Custom Split
Choose split type (1 or 2): 1

Calculating...

Arthur pays: ₹250.00
John pays: ₹250.00
Dutch pays: ₹250.00
Micah pays: ₹250.00
```

## Source Code

The complete program is provided below. You can also find it in [`main.py`](main.py).

```python
"""
Bill Split Calculator
A command-line Python application for splitting shared expenses.
"""


def split_equally(total_amount, people_count):
    """Return each person's equal share, rounded to two decimal places."""
    if people_count <= 0:
        raise ValueError("The number of people must be greater than zero.")
    return round(total_amount / people_count, 2)


def custom_split(people, total_amount):
    """Collect individual contributions and validate their total."""
    while True:
        shares = {}
        total_entered = 0.0

        for person in people:
            while True:
                try:
                    amount = float(input(f"Enter amount paid by {person}: ₹"))
                    if amount < 0:
                        print("Amount cannot be negative. Try again.")
                        continue
                    shares[person] = round(amount, 2)
                    total_entered += amount
                    break
                except ValueError:
                    print("Invalid input. Please enter a valid number.")

        total_entered = round(total_entered, 2)
        total_amount = round(total_amount, 2)

        if total_entered < total_amount:
            print(
                f"\nThe total entered (₹{total_entered}) is LESS than "
                f"the total amount with tip (₹{total_amount})."
            )
            print("Please re-enter the contributions.\n")
        elif total_entered > total_amount:
            print(
                f"\nThe total entered (₹{total_entered}) is MORE than "
                f"the total amount with tip (₹{total_amount})."
            )
            print("Please re-enter the contributions.\n")
        else:
            return shares


def get_positive_amount(prompt):
    """Read a positive amount from the user."""
    while True:
        try:
            amount = float(input(prompt))
            if amount <= 0:
                print("Amount must be greater than 0. Try again.")
                continue
            return amount
        except ValueError:
            print("Invalid input. Please enter a valid number.")


def get_positive_integer(prompt):
    """Read a positive integer from the user."""
    while True:
        try:
            number = int(input(prompt))
            if number <= 0:
                print("Must be at least 1. Try again.")
                continue
            return number
        except ValueError:
            print("Please enter a valid integer.")


def main():
    print("=" * 38)
    print("       BILL SPLIT CALCULATOR")
    print("=" * 38)

    base_amount = get_positive_amount("Enter the total bill amount (₹): ")

    tip_choice = input("Would you like to add a tip? (yes/no): ").strip().lower()
    tip_amount = 0.0

    if tip_choice in ("yes", "y"):
        while True:
            try:
                tip_percent = float(input("Enter tip percentage (e.g., 10 for 10%): "))
                if tip_percent < 0:
                    print("Tip cannot be negative.")
                    continue
                tip_amount = (tip_percent / 100) * base_amount
                print(f"Tip amount added: ₹{round(tip_amount, 2)}")
                break
            except ValueError:
                print("Invalid input. Please enter a number.")
    elif tip_choice not in ("no", "n"):
        print("Invalid choice. Continuing without a tip.")

    total_amount = round(base_amount + tip_amount, 2)
    print(f"\nTotal amount to be split: ₹{total_amount:.2f}")

    people_count = get_positive_integer("Enter number of people: ")
    people = [
        input(f"Enter name of person {index + 1}: ").strip() or f"Person {index + 1}"
        for index in range(people_count)
    ]

    print("\nSplit Type:")
    print("1. Equal Split")
    print("2. Custom Split")
    choice = input("Choose split type (1 or 2): ").strip()

    print("\nCalculating...\n")

    if choice == "1":
        share = split_equally(total_amount, people_count)
        for person in people:
            print(f"{person} pays: ₹{share:.2f}")
    elif choice == "2":
        custom_shares = custom_split(people, total_amount)
        print("Custom Contributions:")
        for person, amount in custom_shares.items():
            print(f"{person} paid: ₹{amount:.2f}")
        print("Custom split matches the total amount.")
    else:
        print("Invalid choice. Please restart the program.")


if __name__ == "__main__":
    main()
```

## Future Enhancements

Possible future improvements include:

- Store previous transactions using SQLite.
- Add a graphical user interface or web interface.
- Support multiple currencies.
- Generate downloadable receipts.
- Categorize expenses.
- Add cloud synchronization and mobile app support.
- Integrate payment services.

These are proposed enhancements; they are not part of the current command-line version.

## Author

**Sasi Dharan M**  
B.E. – Electronics and Communication Engineering  
K. Ramakrishnan College of Engineering

## License

This project is intended for learning and educational use. Add a license file if you want to specify formal reuse and distribution permissions.
