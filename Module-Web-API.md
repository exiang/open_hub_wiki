Wapi support modularization architecture and able to read `protected/modules/YOUR_MODULE/data/api/*.yaml` for swagger definition. `protected/modules/wapi/V1Controller.php` has been modified to auto load `protected/modules/YOUR_MODULE/actions/wapi/V1Controller/*.php` for api action

### Step 1: Define API YAML

### Step 2: Create WAPI Actions