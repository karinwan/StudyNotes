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
```
Create a virutal env named {secend_venv}
```
python3 -m venv venv
```
Activate the Virtual Environment
```
source venv/bin/activate
```
Install required packages
```
pip install -r requirements.txt
```
Start the fastapi server
```
sudo npm run build
sudo npm run dev
```

If any updates on the dependencies
```
cd ./backend
source venv/bin/activate
pip freeze > requirements.txt
```

### ecosci backend Windows
```
cd ./backend
py -3.11 -m venv venv
source venv/Scripts/activate
pip install -r requirements.txt
sudo npm run build
sudo npm run dev
```

If any updates on the dependencies
```
cd ./backend
source venv/Scripts/activate
pip freeze > requirements.txt
```

## Virtual environment with MiniConda:
Download MiniConda on MacOS:
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
```
Install:
```
bash Miniconda3-latest-MacOSX-arm64.sh
```
Miniconda3 will now be installed into this location: `/Users/karinwan/miniconda3`

Restart shell and verify installation
```
source ~/.bashrc  # or source ~/.zshrc (if using Zsh)
conda --version
```

### Update Conda
```
conda update conda
```

### Conda Virtual Environment
- Manages both Python and non-Python dependencies
- can switch Python versions easily
- Uses its own dependency resolver, different from `pip`
```
conda create -n {myenv} python=3.12.8
```
Activates with:
```
conda activate {myenv}
```
Exit with:
```
conda deactivate
```
