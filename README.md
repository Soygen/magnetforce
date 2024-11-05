import math

# Dictionary of magnet grades and their typical Br values in Tesla
MAGNET_GRADES = {
    "N35": 1.23,
    "N38": 1.26,
    "N40": 1.29,
    "N42": 1.32,
    "N45": 1.35,
    "N48": 1.38,
    "N50": 1.43,
    "N52": 1.48,
    "N55": 1.50
}

def calculate_magnet_pull_force(diameter_mm, height_mm, grade):
    """
    Calculate approximate pull force for a cylindrical neodymium magnet
    using simplified magnetic circuit analysis.
    
    Args:
        diameter_mm: Diameter of magnet in mm
        height_mm: Height of magnet in mm
        grade: Magnet grade (e.g., "N52")
    
    Returns:
        pull_force_kg: Approximate pull force in kg
        pull_force_n: Approximate pull force in Newtons
    """
    # Convert dimensions to meters
    diameter = diameter_mm / 1000
    height = height_mm / 1000
    
    # Surface area in m²
    area = math.pi * (diameter/2)**2
    
    # Get Br (Residual Flux Density) for the specified grade
    Br = MAGNET_GRADES[grade]
    
    # Permeability of free space
    mu0 = 4 * math.pi * 1e-7
    
    # Calculate approximate pull force using simplified magnetic circuit equation
    # F = (B²×A)/(2×μ₀)
    # Where B ≈ Br × (t/g)/(1 + t/g)
    # Assuming optimal gap (g) relative to thickness (t)
    B = Br * 0.9  # Approximate working flux density
    
    force_n = (B**2 * area) / (2 * mu0)
    force_kg = force_n / 9.81
    
    return force_kg, force_n

def main():
    print("Neodymium Magnet Pull Force Calculator")
    print("--------------------------------------")
    
    while True:
        try:
            diameter = float(input("\nEnter magnet diameter (in mm): "))
            height = float(input("Enter magnet height (in mm): "))
            
            if diameter <= 0 or height <= 0:
                print("Error: Dimensions must be positive numbers")
                continue
            
            # Display available grades
            print("\nAvailable magnet grades:")
            for grade in sorted(MAGNET_GRADES.keys()):
                print(f"  {grade} (Br = {MAGNET_GRADES[grade]} Tesla)")
            
            # Get magnet grade
            while True:
                grade = input("\nEnter magnet grade (e.g., N52): ").upper()
                if grade in MAGNET_GRADES:
                    break
                print("Error: Invalid grade. Please choose from the list above.")
            
            pull_force_kg, pull_force_n = calculate_magnet_pull_force(diameter, height, grade)
            
            print(f"\nEstimated pull force for {diameter}x{height}mm {grade} magnet:")
            print(f"  {pull_force_kg:.2f} kg")
            print(f"  {pull_force_n:.2f} N")
            
            another = input("\nCalculate another? (y/n): ").lower()
            if another != 'y':
                break
                
        except ValueError:
            print("Error: Please enter valid numbers")

if __name__ == "__main__":
    main()
