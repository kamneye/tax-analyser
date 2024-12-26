# tax-analyser
import pandas as pd
import numpy as np
def create_tax_bracket_dataframe():
    lower_limits = []
    upper_limits = []
    tax_rates = []
    
    while True:
        # Get user input for each tax bracket
        lower_limit = float(input("Enter the lower limit of the tax bracket (in $): "))
        upper_limit = float(input("Enter the upper limit of the tax bracket (in $): "))
        tax_rate = float(input("Enter the tax rate for this bracket (in %): "))
        
        # Append the values to the respective lists
        lower_limits.append(lower_limit)
        upper_limits.append(upper_limit)
        tax_rates.append(tax_rate)
        
        # Ask the user if they want to add another tax bracket
        more_brackets = input("Do you want to add another tax bracket? (y/n): ").strip().lower()
        if more_brackets != 'y':
            break
    
    # Create and return the DataFrame
    tax_brackets_df = pd.DataFrame({
        'lower_limit': lower_limits,
        'upper_limit': upper_limits,
        'tax_rate': tax_rates
    })
    
    return tax_brackets_df
def calculate_tax(income, tax_brackets_df):
    tax_owed = 0.0
    
    # Loop through each bracket and calculate tax
    for _, row in tax_brackets_df.iterrows():
        if income > row['lower_limit']:
            taxable_income = min(income, row['upper_limit']) - row['lower_limit']
            tax_owed += taxable_income * (row['tax_rate'] / 100)
    
    return tax_owed
def main():
    # Get the user's income
    income = float(input("Enter your total income (in $): "))
    
    # Create the tax brackets DataFrame based on user input
    tax_brackets_df = create_tax_bracket_dataframe()
    
    # Calculate the tax owed based on the provided tax brackets
    tax_owed = calculate_tax(income, tax_brackets_df)
    
    # Display the tax calculation breakdown
    print("\nTax Calculation Breakdown:")
    print(f"Income: ${income}")
    print("\nTax Brackets:")
    print(tax_brackets_df)
    
    print(f"\nTotal Tax Owed: ${tax_owed:.2f}")
if __name__ == "__main__":
    main()
