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



Data visualization 
------------------

Graph 1
++++++++++++++++++
We have two graphs, first is top states by deaths due to violence and top states by injuries due to violence. California is on the first position in deaths but Illinois is on the first position in injuries.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_19_0.png?raw=true

Graph 2
++++++++++++++++++
In this plot, we show the lowest 15 states with casualties
those are the states that have fewer deaths and injuries frim gun violence. The lowest states are Wyoming, Vermont an Hawaii.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_21_1.png?raw=true


Graph 3
++++++++++++++++++
The following plots show the correlations between the columns, represented in scatter plots,from which we can see the correlation between each two columns in our dataset There are interesting correlations in our data starting by the correlations with total number of incidents, which seem to have positive correlation with almost all the other variables particularly number of deaths, injuries, mass shooting, and unintentional shooting. This show that in most of our data there is a relationship between the number of incidents that occurred in each state with the other variables being an important factor in those incidents. Then, the number of injuries and deaths also have positive linear correlations with a mass shooting, home invasion, and unintentional shooting. Which also makes sense when we talk about gun violence all those factors are related. Mass shooting is also interesting variable as it has positive linear relationship with total number of incidents, injuries and death this happen because the increase in mass shooting will increase those variables there are other noticeable linear relationships such as the relationship between home invasions and unintentional shooting, which means that one of the reasons of unintentional shooting is the invasion of houses where people tend to react by shooting the people who try enter their houses illegally. Also, there is I testing the positive linear relationship between officers involved in incidents and mass shooting, which means that the more involvement of the police there tend to be a more mass shooting. In general, we tried to show that these dataset variables have a lot of positive correlations that can explain some of the main reasons for gun violence in the united states.


.. figure:: https://raw.githubusercontent.com/beka10/DS320FinalProject/master/_build/html/_images/projectfinal_21_0.png

Graph 4
+++++++++++++
Here we show the map of the United States with the highest places (cities) with gun violence we see that it is mostly spread in the east, west coast and the south. With red points almost everywhere except the center areas of the country.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_41_1.png?raw=true


Graph 5
+++++++++++++

This map shows the number of incidents of gun violence in the state of Iowa. As we can see from the map there is more concentration of red dots on the bigger cities with more population like Des Moines, Iowa City, and Cedar Rapids The same happens when we look at Illinois, there is a lot of red dots around Chicago comparing to the rest of the state. This also confirmed the trend that we have been seeing the big cities in the states with the most incidents of gun violence.


.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_43_0.png?raw=true


Graph 6
+++++++++++++++

This plot shows home invasion per state. We can see that some trends continue we still have Florida, and Texas in the top 4 states with home invasion, which indicate that home invasions might be an important reason of incidents, deaths, and injuries from gun violence in the United States. Also, We can see that Georgia is in the top 4 of home invasions but it was not in the highest states with the highest incidents and deaths.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_23_0.png?raw=true


Graph 7
++++++++++++++

In this plot, we look at the top 10 states of the overall violence. Overall violence is the sum of the number of deaths, injuries, incidents mass shooting, officers involved and unintentional shooting We can see that California, Texas, Florida and Illinois are still the top four states in overall violence.


.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_24_1.png?raw=true


Graph 8
++++++++++++++

This plot shows the number of teenagers involved in gun violence per state the teenagers here are under the age of 17 As we can see from the plot below Illinois has the highest number of teenagers involved in gun violence followed by Florida, California and Texas those are the same 4 highest states for incidents and deaths from gun violence this prove that Today’s children and teenagers are more prone to violence.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_27_2.png?raw=true



Graph 9
+++++++++++++

This plot shows the relationship between a mass shooting and total deaths As we can see from the plot mostly as the mass shootings increase the number of deaths will increase. According to the US Congressional Research Service mass shooting is four or more shot and/or killed in a single event (incident) this shows that mass shootings are incidents with more people being killed or injured.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_31_0.png?raw=true


Graph 10
++++++++++++++
Now we move from states and look at the cities with the highest
gun violence in the United States. We can see that Chicago, Baltimore 
Washington DC is the highest cities in gun violence.
it is interesting that there are only two cities from
the top four states, which are Chicago and Houston.


.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_40_1.png?raw=true


Graph 11
+++++++++++++++
Here we plot gun violence incidents per year in the past 4 years
we can see that gun violence is increasing every year
in the past 4 years to reach 61000 incidents in 2017.
This year so far there were 13000 incidents, this counts only
till March 31st, 2018.

.. figure:: https://github.com/beka10/DS320FinalProject/blob/master/_build/html/_images/projectfinal_47_0.png?raw=true

Python Notebook with more data visualization graphs
----------------------
In this part, you can download our Python Notebook where we have uploaded all the visualization graphs that we did for this project. You will see more than 20 interesting graphs and all of them can provide a good analysis.





Conclusion
---------------------
Overall, from our visualization, we notice that gun violence is spread throughout the whole country except for the middle region. There are regions with the most gun violence incidents such as Illinois, California, Texas, and Florida. Those are some of the most populated states in the United States. Which might indicate that the more population of the state the more incidents and gun violence in that state. In those states, gun violence is more in the big cities than the rest of the state which is due to the higher population there. But, it can also be explained with other reasons and factors like the increase in gun sales in those states. 91% of the incidents happen with one gun, which might indicate that most of the incidents are mass shootings, unintentional shootings or big attacks to certain areas. So, the sale and availability of guns are one of the main reasons for the gain violence increase in the US in the past five years according to our data. In addition, the increase of home invasions in those states is one of the reasons for the gun violence increase. Home invasions are highly correlated with an unintentional shooting which might be the direct reason for the increase of unintentional shooting in these states. Then, we noticed that there is an increase in the number of children involved and killed in gun violence in the past years especially in Chicago. This might be due to the many reasons such as violent video games. Thus, our data visualization showed the locations and distribution of gun b=violence in the United States and some of the reason like gun sales, children involvement and the increase of home invasions. 
In the future, we would like to continue working on this project and try to join our datasets with other datasets to explain more reasons for gun violence. We would like to look for gun violence per capita or per 100000 people to see the rate of gun violence regardless of the high population. Also, we would like to look more into the political reasons such as the congressional reasons that voted against gun violence laws and the effect of that in their congregational districts. 

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
