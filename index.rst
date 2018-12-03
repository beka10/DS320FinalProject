.. My Data Visualization Project documentation master file, created by
   sphinx-quickstart on Thu Nov  1 11:25:28 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to our Data Visualization Project's documentation!
=========================================================
Beka Beriashvili and Salamu Ali Brahim

This is our project.

Gun Violence
-----------------
.. figure:: https://cdn.vox-cdn.com/thumbor/rpnUZf58x66rdd0oyXHgLcTb2uI=/0x0:2040x1360/1200x800/filters:focal(860x538:1186x864)/cdn.vox-cdn.com/uploads/chorus_image/image/59142891/acastro_171108_1777_0009_v2.0.jpg


Introduction
-----------------

Welcome to data visualization of gun violence in the United States using data from multiple sources. Gun violence results in ten thousands of deaths and injuries annually in the United States. last year,  there were more than 300 mass shootings and thousands of people killed and injured . Our purpose is to find the states with the most and least gun violence, and what are the reasons and factors that increase gun violence in the United States? To answer this question we gathered data from two sources, data from gunviolencearchive.org and gun violence in the US data from Kaggle.com. To get the dataset from gunviolencearchive.org we had to parse the website to get the data for the 50 states. It  has the following variables for all fifty states: State, Total Number of Incidents, Number of Deaths, Number of Injuries, Number of Children , Number of Teens, Mass Shooting, Officer Involved  Incident, Home Invasion, Defensive Use and Unintentional Shooting. In addition, we cleaned the Kaggle.com dataset and removed the columns that were missing many values to end up with 239677 rows of data for the following variables: incident id, date, state, city , number of people killed, number of people injured, congressional district, latitude, longitude, number of guns involved. Both of the datasets will be used in our visualization in order to investigate the results and find answers to our questions. 
This website will outline all the details of this project that includes the data acquisition, data visualization, background information and a narrative that will provide answers to answers our initial questions.  




Parsing Data with Python
-----------------
We found our dataset on https://www.gunviolencearchive.org/



In order to run this code below and make csv file you need to download this html materials. We downloaded it manually because it was blocked and we could not get the access. 

*  `Gun violence zip files <http://knuth.luther.edu/~beribe01/gunviolence.zip>`_ 



.. code-block:: python

	def main():
    	   # to import libraries
    	   import ssl
    	   from bs4 import BeautifulSoup as bto
    	   import csv

    	   # to create default context
    	   ctx = ssl.create_default_context()
    	   ctx.check_hostname = False
    	   ctx.verify_mode = ssl.CERT_NONE

    	   # we will use this list for loop in order to get the data for each state
    	   list_states = ["Alabama","Alaska","Arizona","Arkansas","California","Colorado",
    	   "Connecticut","Delaware","Florida","Georgia","Hawaii","Idaho","Illinois",
    	   "Indiana","Iowa","Kansas","Kentucky","Louisiana","Maine","Maryland",
    	   "Massachusetts","Michigan","Minnesota","Mississippi","Missouri","Montana",
    	   "Nebraska","Nevada","New Hampshire","New Jersey","New Mexico","New York",
    	   "North Carolina","North Dakota","Ohio","Oklahoma","Oregon","Pennsylvania",
    	   "Rhode Island","South Carolina","South Dakota","Tennessee","Texas","Utah",
    	   "Vermont","Virginia","Washington","West Virginia","Wisconsin","Wyoming"]

    	   # We used beautiful soup in order to parse the data. We found tag name and appended data that we wanted
    	   # to a list
    	   all_states = []
    	   for i in list_states:
        	   url = "/Users/Beka/Downloads/gunviolence/" + i + ".html"
        	   file = open(url, "r")
        	   soup = bto(file, 'html.parser')
        	   country_codes=[]
        	   for tag in soup.find_all('span'):
            	   country_codes.append(tag.text)
        	   ccodes = country_codes.index('Total Number of Incidents')
        	   country_codes = country_codes[ccodes:ccodes + 22]
        	   country_codes.insert(0, i)
        	   country_codes.insert(0, "State")
        	   all_states.append(country_codes)
    	   # we are making dictionaries with keys and values. Keys will be columns and 
    	   # values will be values for each state
    	   states = []
    	   keys=[]
    	   values=[]
    	   for i in all_states:
        	   for i,j in enumerate(i):
            	   if i%2==0:
                	   keys.append(j)
            	   else:
                	   values.append(j)
        	   dict1 = dict(zip(keys,values))
        	   states.append(dict1)


    	   # created list for columns
    	   columns = ['State', 'Total Number of Incidents', 'Number of Deaths1',
    	   'Number of Injuries1', 'Number of Children (age 0-11)', 'Number of Teens (age 12-17)',
    	   'Mass Shooting2', 'Officer Involved Incident', 'Home Invasion2', 'Defensive Use2',
    	   'Unintentional Shooting2']
    	   # we are making csv file for our dataset
    	   csv_file = "violence.csv"
    	   try:
        	   with open(csv_file, 'w') as csvfile:
            	   writer = csv.DictWriter(csvfile, fieldnames=columns)
            	   writer.writeheader()
            	   for data in states:
                	   writer.writerow(data)
    	   except IOError:
        	   print("I/O error") 
    	   print('CSV file "violence.csv" is created')    
      
	if __name__=='__main__':
    	   main() 

`Click here and download the python file of the code above  <http://knuth.luther.edu/~beribe01/FinalProject.py>`_ 

The dataset after parsing and cleaning.


.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/picture.png?raw=true



You can download this data set from here.

`http://knuth.luther.edu/~beribe01/gunviolence.csv <http://knuth.luther.edu/~beribe01/gunviolence.csv>`_ 



Data Visualization
------------------

We have two graphs, first is top states by deaths due to violence and top states by injuries due to violence. California is on the first position in deaths but Illinois is on the first position in injuries


.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_19_0.png?raw=true




+++++++++++++++++

.. toctree::
   :maxdepth: 2
   :caption: Contents:
   :ref: 'projectfinal.ipynb'



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
