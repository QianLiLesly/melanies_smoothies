# Import python packages
import streamlit as st
from snowflake.snowpark.functions import col # instead of the whole table

# Write directly to the app
st.title("Customize Your Smoothie :cup_with_straw:")
st.write(
    """Choose the fruits you want in your custom Smoothie!
    """
)

# Gather Customers' Name

name_on_order = st.text_input("Name on Smoothie:")
st.write("The name on your smoothie would be:", name_on_order)

# Menu table from internal dataset
cnx = st.connection("snowflake")
session = cnx.session()

my_dataframe = session.table("smoothies.public.fruit_options").select(col('FRUIT_NAME'))
# st.dataframe(data=my_dataframe, use_container_width=True)

ingredients_list = st.multiselect(
    "Choose up to 5 indgredients:",
    my_dataframe,
    max_selections = 5
)

# st.write(ingredients_list)
# st.text(ingredients_list)
            
if ingredients_list:
    ingredients_string = '' 
    
    # create a variable that makes PY thinking it contains a string
    # for loop for repeating the process   

    for frutis_choosen in ingredients_list:
        ingredients_string += frutis_choosen+' '

#st.write(ingredients_string)

    my_insert_stmt = """ insert into smoothies.public.orders(ingredients,name_on_order)
                values('"""+ingredients_string+"""','"""+name_on_order+"""')"""

#st.write(my_insert_stmt)
#st.stop()

    time_to_insert = st.button('Submit Order')

    if time_to_insert:
        session.sql(my_insert_stmt).collect()
        st.success('Your Smoothie is ordered, '+name_on_order+' !', icon="✅")
