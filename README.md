# pymongo_learn

import pymongo

client = pymongo.MongoClient("10.0.1.40", 27017, username="Marek", password="Mongo2021")

opk_list = ["opk1", "opk2", "opk3", "opk4", "opk5", "opk6", "opk7"]


# dokończyć wprowadzanie, utworzyć wyjątki i wyłapywanie błędów jeśli jest różny od cyfry
print('\nproszę podać miesiąc cyfrą a rok w całoście np. "07", "2019"')
month = input('proszę podać miesiąc do wykonania analizy: ')
year = input('proszę podać rok do wykonania analizy: ')
analisPeriod = str(month + '-' + year)

dbPharmaMaterialOut = client['Apteka_rozchody_' + analisPeriod]
col_OPK_list = dbPharmaMaterialOut['OPK_list']
col_cost_list = dbPharmaMaterialOut['Rodzaje_kosztów_list']

numb_doc = 0

for i in col_OPK_list.find({}, {"miesiac": analisPeriod, "List_OPK":1}):
    numb_doc += 1


if numb_doc ==1:
    print("-------")
    print("!!!!!!! analizowany m-c jest już wgrany !!!!!!!")
    print("-------")
elif numb_doc > 1:
    print("-------")
    print("!!!!!!! UWAGA ZDUBLOWANE DANE !!!!!!!")
    print("-------")
else:
    dict_OPK_list = {"miesiac": analisPeriod, "List_OPK": opk_list}
    doc = col_OPK_list.insert_one(dict_OPK_list)
