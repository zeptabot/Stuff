```python
import ipywidgets as widgets
from IPython.display import display, HTML, FileLink
import pandas as pd
import os

# CSV file name
csv_file = "protein_and_fat_data.csv"

# Load data from CSV file on startup
def load_data_from_csv():
    global protein_items, fat_source
    if os.path.exists(csv_file):
        df = pd.read_csv(csv_file)
        # Load protein items
        protein_items = df[df['Type'] == 'Protein'].to_dict('records')
        # Load fat source
        fat_source = df[df['Type'] == 'Fat Source'].to_dict('records')[0] if not df[df['Type'] == 'Fat Source'].empty else None
        update_ranked_list()
    else:
        protein_items = []
        fat_source = None

# Save data to CSV file
def save_data_to_csv():
    protein_df = pd.DataFrame(protein_items)
    fat_source_df = pd.DataFrame([fat_source]) if fat_source else pd.DataFrame()
    
    # Add 'Type' column to distinguish protein items from fat source
    if not protein_df.empty:
        protein_df['Type'] = 'Protein'
    if not fat_source_df.empty:
        fat_source_df['Type'] = 'Fat Source'
    
    # Combine both into one CSV file
    combined_df = pd.concat([protein_df, fat_source_df], ignore_index=True)
    
    # Write to CSV
    combined_df.to_csv(csv_file, index=False)

# Function to calculate the cost for 30g of protein for a single item
def cost_per_30g_protein(unit_price, unit_weight, serving_weight, protein_per_serving):
    try:
        cost = (30 * unit_price * serving_weight) / (protein_per_serving * unit_weight)
        return cost
    except ZeroDivisionError:
        return float('inf')

# Function to calculate the fat that comes with 30g of protein
def fat_with_30g_protein(protein_per_serving, fat_per_serving, serving_weight):
    try:
        fat_per_gram_of_protein = fat_per_serving / protein_per_serving
        fat_with_30g_protein = fat_per_gram_of_protein * 30
        return fat_with_30g_protein
    except ZeroDivisionError:
        return 0

# Function to calculate cost per gram of fat from the chosen fat source
def cost_per_gram_of_fat(fat_price, fat_unit_weight, fat_serving_weight, fat_per_serving):
    return fat_price / (fat_per_serving * (fat_unit_weight / fat_serving_weight))

# Function to calculate the adjusted cost of 30g protein (accounting for fat savings)
def adjusted_cost_of_30g_protein(cost_per_30g_protein, fat_with_30g_protein, cost_per_gram_fat):
    return cost_per_30g_protein - (fat_with_30g_protein * cost_per_gram_fat)

# Function to add a new protein item and update the rankings
def add_protein_item(b):
    name = name_input.value
    seller_brand = seller_brand_input.value

    try:
        unit_price = float(price_input.value) if price_input.value else 0.0
        unit_weight = float(weight_input.value) if weight_input.value else 0.0
        serving_weight = float(serving_input.value) if serving_input.value else 0.0
        protein_per_serving = float(protein_input.value) if protein_input.value else 0.0
        fat_per_serving = float(fat_input.value) if fat_input.value else 0.0
    except ValueError:
        with ranked_output:
            ranked_output.clear_output()
            display(HTML("<b>Error: Please enter valid numerical values!</b>"))
        return

    product_link = link_input.value
    additional_notes = notes_input.value

    cost_30g_protein = cost_per_30g_protein(unit_price, unit_weight, serving_weight, protein_per_serving)
    fat_with_30g_protein_value = fat_with_30g_protein(protein_per_serving, fat_per_serving, serving_weight)

    # Add the item as a dictionary to the list
    item = {
        'name': name,
        'seller_brand': seller_brand,
        'unit_price': unit_price,
        'unit_weight': unit_weight,
        'serving_weight': serving_weight,
        'protein_per_serving': protein_per_serving,
        'fat_per_serving': fat_per_serving,
        'cost_per_30g_protein': cost_30g_protein,
        'fat_with_30g_protein': fat_with_30g_protein_value,
        'product_link': product_link,
        'additional_notes': additional_notes
    }
    protein_items.append(item)

    # Save data to CSV and update rankings
    save_data_to_csv()
    update_ranked_list()

# Function to update the ranked list of items (both normal and adjusted)
def update_ranked_list():
    # Clear previous content
    ranked_output.clear_output()
    adjusted_ranked_output.clear_output()

    # Sort the protein items by cost per 30g protein (ascending order)
    protein_items.sort(key=lambda x: x['cost_per_30g_protein'])

    # Display the first ranked list (normal cost per 30g protein)
    with ranked_output:
        display(HTML("<b>Ranked List of Items by Cost per 30g Protein (Cheapest to Most Expensive):</b>"))
        for i, item in enumerate(protein_items, start=1):
            display(HTML(f"{i}. {item['name']} - ${item['cost_per_30g_protein']:.2f} per 30g protein - "
                         f"Seller/Brand: {item['seller_brand']} - "
                         f"<a href='{item['product_link']}' target='_blank'>Link</a> - "
                         f"Notes: {item['additional_notes']}"))

    # Display the second ranked list (adjusted cost per 30g protein considering fat savings)
    if fat_source:
        cost_per_gram_fat = cost_per_gram_of_fat(fat_source['fat_price'], fat_source['fat_unit_weight'], fat_source['fat_serving_weight'], fat_source['fat_per_serving'])
        for item in protein_items:
            item['adjusted_cost'] = adjusted_cost_of_30g_protein(item['cost_per_30g_protein'],
                                                                 item['fat_with_30g_protein'],
                                                                 cost_per_gram_fat)
        # Sort by adjusted cost per 30g protein (ascending order)
        protein_items.sort(key=lambda x: x['adjusted_cost'])

        with adjusted_ranked_output:
            display(HTML("<b>Ranked List of Items by Adjusted Cost (with fat savings, Cheapest to Most Expensive):</b>"))
            for i, item in enumerate(protein_items, start=1):
                display(HTML(f"{i}. {item['name']} - Adjusted Cost: ${item['adjusted_cost']:.2f} - "
                             f"Seller/Brand: {item['seller_brand']} - "
                             f"<a href='{item['product_link']}' target='_blank'>Link</a> - "
                             f"Notes: {item['additional_notes']}"))

    # Save the rankings to the CSV file
    save_data_to_csv()

# Function to update the fat source (only one fat source at a time)
def update_fat_source(b):
    fat_name = fat_name_input.value
    fat_seller = fat_seller_input.value

    try:
        fat_unit_weight = float(fat_unit_weight_input.value) if fat_unit_weight_input.value else 0.0
        fat_serving_weight = float(fat_serving_weight_input.value) if fat_serving_weight_input.value else 0.0
        fat_per_serving = float(fat_per_serving_input.value) if fat_per_serving_input.value else 0.0
        fat_price = float(fat_price_input.value) if fat_price_input.value else 0.0
    except ValueError:
        with fat_output:
            fat_output.clear_output()
            display(HTML("<b>Error: Please enter valid numerical values!</b>"))
        return

    fat_link = fat_link_input.value

    global fat_source
    fat_source = {
        'fat_name': fat_name,
        'fat_seller': fat_seller,
        'fat_unit_weight': fat_unit_weight,
        'fat_serving_weight': fat_serving_weight,
        'fat_per_serving': fat_per_serving,
        'fat_price': fat_price,
        'fat_link': fat_link
    }

    # Update the fat output area
    with fat_output:
        fat_output.clear_output()
        display(HTML(f"<b>Current Fat Source:</b> {fat_name} - Seller: {fat_seller} - "
                     f"<a href='{fat_link}' target='_blank'>Link</a> - "
                     f"Cost per Gram of Fat: ${cost_per_gram_of_fat(fat_price, fat_unit_weight, fat_serving_weight, fat_per_serving):.2f}"))

    # Save data to CSV and update rankings
    save_data_to_csv()
    update_ranked_list()

# Function to download the CSV file to the user's local device
def download_csv_file(b):
    display(FileLink(csv_file, result_html_prefix="Click here to download: "))

# Initialize the lists for protein items and fat source
protein_items = []
fat_source = None

# Input fields for protein items
name_input = widgets.Text(description="Item Name:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
seller_brand_input = widgets.Text(description="Seller/Brand:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
price_input = widgets.Text(description="Unit Price ($):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
weight_input = widgets.Text(description="Unit Weight (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
serving_input = widgets.Text(description="Serving Weight (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
protein_input = widgets.Text(description="Protein per Serving (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_input = widgets.Text(description="Fat per Serving (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
link_input = widgets.Text(description="Product Link:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
notes_input = widgets.Text(description="Additional Notes:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})

# Add item button
add_button = widgets.Button(description="Add Protein Item")
add_button.on_click(add_protein_item)

# Input fields for fat source
fat_name_input = widgets.Text(description="Fat Source Name:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_seller_input = widgets.Text(description="Seller/Brand:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_unit_weight_input = widgets.Text(description="Unit Weight (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_serving_weight_input = widgets.Text(description="Serving Weight (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_per_serving_input = widgets.Text(description="Fat per Serving (g):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_price_input = widgets.Text(description="Unit Price ($):", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})
fat_link_input = widgets.Text(description="Product Link:", layout=widgets.Layout(width='400px'), style={'description_width': '200px'})

# Update fat source button
update_fat_button = widgets.Button(description="Update Fat Source")
update_fat_button.on_click(update_fat_source)

# Download CSV button
download_csv_button = widgets.Button(description="Download CSV")
download_csv_button.on_click(download_csv_file)

# Create HBox containers for protein and fat inputs to display them side by side
protein_input_box = widgets.VBox([name_input, seller_brand_input, price_input, weight_input, serving_input, protein_input, fat_input, link_input, notes_input, add_button])
fat_input_box = widgets.VBox([fat_name_input, fat_seller_input, fat_unit_weight_input, fat_serving_weight_input, fat_per_serving_input, fat_price_input, fat_link_input, update_fat_button])

# Display protein and fat input fields side by side
input_boxes = widgets.HBox([protein_input_box, fat_input_box])

# Output areas for ranked lists and fat source information
ranked_output = widgets.Output()
adjusted_ranked_output = widgets.Output()
fat_output = widgets.Output()

# Display the input boxes and outputs
display(input_boxes, download_csv_button)
display(ranked_output, adjusted_ranked_output, fat_output)

# Load data from CSV on startup
load_data_from_csv()

```


    HBox(children=(VBox(children=(Text(value='', description='Item Name:', layout=Layout(width='400px'), style=Tex…



    Button(description='Download CSV', style=ButtonStyle())



    Output()



    Output()



    Output()



```python

```


```python

```
