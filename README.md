# Air_Quality_in_Madrid
Different pollution levels in Madrid

Data available from Kaggle:
https://www.kaggle.com/decide-soluciones/air-quality-madrid/

Due to similarity, only data from year 2017 will be analyzed.


Context
In the recent years, the high levels of pollution during certain dry periods in Madrid has forced the authorities to take measures against the use of cars in the city center, and has been used as a reson to propose drastic modifications in the city's urbanism. Thanks to Madrid's City Council Open Data website, the air quality data has been uploaded is plubicly available. There are several files available, including daily and hourly historical data of the levels registered from 2001 to 2018 and the list of stations being used for pollution and other particles analysis in the city.

However, when exploring this data from a data analysis and time series point of view, we found that the format was somehow confusing and not common, and some design decisions in the dataset were far from optimal: The hourly data was split in monthly files containing slightly different formats through the years, which were equally as uncommon: rows are certain measures in certain days, each containing 24 columns (one per hour in the day) that includes a control character. This control character is V if the measurement is valid, and mostly (but not exclusively) N if not.

These handicaps when exploring the historical data can ruin the purpose of the Open Data: to be publicly audited, and to be freely explored and used for experimentation. For that reason in Decide we are release our own version of the data, which has been designed for ease of use using common standards and performant formats. This allows to ship a faster, smaller and more convenient and intuitively structured dataset.

Content
All the data is extracted from the original files and processed to result in a more convenient format for typical Kaggle purposes. While the original data includes hours as different columns and measurements as different rows, this version is structured the other way round: Each row is timestamped and the columns are the different measures performed at that point in time in a certain stations. This allows faster preparation for time series analysis and prediction tasks.

This dataset defines stations as the higher hierarchical level: each individual station history can be individually extracted from the file for further study. Inside each station's DataFrame, all the particles measurements that such station has registered in the period of 2001/01 - 2018/04 (if active this whole time). Not every station has the same equipment, therefore each station can measure only a certain subset of particles. The complete list of possible measurements and their explanations (following the original explanation document) are:

SO_2: sulphur dioxide level measured in μg/m³. High levels of sulphur dioxide can produce irritation in the skin and membranes, and worsen asthma or heart diseases in sensitive groups.
CO: carbon monoxide level measured in mg/m³. Carbon monoxide poisoning involves headaches, dizziness and confusion in short exposures and can result in loss of consciousness, arrhythmias, seizures or even death in the long term.
NO: nitric oxide level measured in μg/m³. This is a highly corrosive gas generated among others by motor vehicles and fuel burning processes.
NO_2: nitrogen dioxide level measured in μg/m³. Long-term exposure is a cause of chronic lung diseases, and are harmful for the vegetation.
PM25: particles smaller than 2.5 μm level measured in μg/m³. The size of these particles allow them to penetrate into the gas exchange regions of the lungs (alveolus) and even enter the arteries. Long-term exposure is proven to be related to low birth weight and high blood pressure in newborn babies.
PM10: particles smaller than 10 μm. Even though the cannot penetrate the alveolus, they can still penetrate through the lungs and affect other organs. Long term exposure can result in lung cancer and cardiovascular complications.
NOx: nitrous oxides level measured in μg/m³. Affect the human respiratory system worsening asthma or other diseases, and are responsible of the yellowish-brown color of photochemical smog.
O_3: ozone level measured in μg/m³. High levels can produce asthma, bronchytis or other chronic pulmonary diseases in sensitive groups or outdoor workers.
TOL: toluene (methylbenzene) level measured in μg/m³. Long-term exposure to this substance (present in tobacco smkoke as well) can result in kidney complications or permanent brain damage.
BEN: benzene level measured in μg/m³. Benzene is a eye and skin irritant, and long exposures may result in several types of cancer, leukaemia and anaemias. Benzene is considered a group 1 carcinogenic to humans by the IARC.
EBE: ethylbenzene level measured in μg/m³. Long term exposure can cause hearing or kidney problems and the IARC has concluded that long-term exposure can produce cancer.
MXY: m-xylene level measured in μg/m³. Xylenes can affect not only air but also water and soil, and a long exposure to high levels of xylenes can result in diseases affecting the liver, kidney and nervous system (especially memory and affected stimulus reaction).
PXY: p-xylene level measured in μg/m³. See MXY for xylene exposure effects on health.
OXY: o-xylene level measured in μg/m³. See MXY for xylene exposure effects on health.
TCH: total hydrocarbons level measured in mg/m³. This group of substances can be responsible of different blood, immune system, liver, spleen, kidneys or lung diseases.
CH4: methane level measured in mg/m³. This gas is an asphyxiant, which displaces the oxygen animals need to breath. Displaced oxygen can result in dizzinnes, weakness, nausea and loss of coordination.
NMHC: non-methane hydrocarbons (volatile organic compounds) level measured in mg/m³. Long exposure to some of these substances can result in damage to the liver, kidney, and central nervous system. Some of them are suspected to cause cancer in humans.
Also the master DataFrame is included the file, which contains information about the active stations. Notice that only active stations are included in there, since the Open Data files do not provide information about the stations that have ceased activity.

Using this hierarchical structure, we can store it in an HDF5 file, which is also compressed and allows for great performance when accessing contiguous data (which is the casa in this time-indexed design). These modifications allow to encapsulate the same information that is provided in the original page in monthly files adding up to 250MiB in a single, structured file of just 74MiB. Since some people may not be familiar with HDF5 format yet, we provide some snippets to make it easier for you to start exploring the data in Python. You can find a short introduction in to HDF5 format in this kernel.

However, if for some reason using HDF5 is still inconvenient for you, this dataset also provides a zip folder containing the same information gathered in plain-text CSV files and a stations.csv file equivalent to the master dataframe. These CSV files still benefit from the data reorganization but the lack of advatange performances make them much heavier (174MiB compressed, 500MiB uncompressed).
