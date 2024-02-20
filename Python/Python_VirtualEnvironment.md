## Virtual Environment
```
virtualenv vnev
```
```
source plotting/bin/active
```
Deactivate a virtual environment:
``` 
deactivate
```

### ecosci backend MAC
```
cd ./backend
# Create a virutal env named {secend_venv}
python3 -m venv venv
# Activate the Virtual Environment
source venv/bin/activate
# install required packages
pip install -r requirements.txt
# start the fastapi server
sudo npm run build
sudo npm run dev
```
```
# if any updates on the dependencies
cd ./backend
# Activate the Virtual Environment
source venv/bin/activate
pip freeze > requirements.txt
```

### ecosci backend Windows
```
cd ./backend
# Create a virutal env named {secend_venv}
py -3.11 -m venv venv
# Activate the Virtual Environment
source venv/Scripts/activate
# install required packages
pip install -r requirements.txt
# start the fastapi server
sudo npm run build
sudo npm run dev
```
```
# if any updates on the dependencies
cd ./backend
# Activate the Virtual Environment
source venv/Scripts/activate
pip freeze > requirements.txt
```
