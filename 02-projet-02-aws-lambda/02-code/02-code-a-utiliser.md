# Projet "To the Power of Math!"

Ce projet utilise AWS pour créer une application web capable de calculer des puissances mathématiques. Il comprend une interface utilisateur simple qui permet d'entrer une base et un exposant, et utilise AWS Lambda pour effectuer les calculs.

## Contenu du Projet

Le projet est composé des fichiers suivants avec leur contenu détaillé ci-dessous :

### 1. `index-ORIGINAL.html`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>To the Power of Math!</title>
</head>
<body>
    To the Power of Math!
</body>
</html>
```

### 2. `PowerOfMathFunction - Lambda-ORIGINAL.txt`

```python
# import the JSON utility package
import json
# import the Python math library
import math

# define the handler function that the Lambda service will use as an entry point
def lambda_handler(event, context):

    # extract the two numbers from the Lambda service's event object
    mathResult = math.pow(int(event['base']), int(event['exponent']))

    # return a properly formatted JSON object
    return {
        'statusCode': 200,
        'body': json.dumps('Your result is ' + str(mathResult))
    }
```

### 3. `Execution Role Policy JSON.txt`

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "dynamodb:PutItem",
                "dynamodb:DeleteItem",
                "dynamodb:GetItem",
                "dynamodb:Scan",
                "dynamodb:Query",
                "dynamodb:UpdateItem"
            ],
            "Resource": "YOUR-TABLE-ARN"
        }
    ]
}
```

### 4. `PowerOfMathFunction - Lambda-FINAL.txt`

```python
# import the JSON utility package
import json
# import the Python math library
import math

# import the AWS SDK (for Python the package name is boto3)
import boto3
# import two packages to help us with dates and date formatting
from time import gmtime, strftime

# create a DynamoDB object using the AWS SDK
dynamodb = boto3.resource('dynamodb')
# use the DynamoDB object to select our table
table = dynamodb.Table('PowerOfMathDatabase')
# store the current time in a human readable format in a variable
now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

# define the handler function that the Lambda service will use as an entry point
def lambda_handler(event, context):

    # extract the two numbers from the Lambda service's event object
    mathResult = math.pow(int(event['base']), int(event['exponent']))

    # write result and time to the DynamoDB table using the object we instantiated and save response in a variable
    response = table.put_item(
        Item={
            'ID': str(mathResult),
            'LatestGreetingTime': now
        }
    )

    # return a properly formatted JSON object
    return {
        'statusCode': 200,
        'body': json.dumps('Your result is ' + str(mathResult))
    }
```

### 5. `index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>To the Power of Math!</title>
    <!-- Styling for the client UI -->
    <style>
        h1 {
            color: #FFFFFF;
            font-family: system-ui;
            margin-left: 20px;
        }
        body {
            background-color: #222629;
        }
        label {
            color: #86C232;
            font-family: system-ui;
            font-size: 20px;
            margin-left: 20px;
            margin-top: 20px;
        }
        button {
            background-color: #86C232;
            border-color: #86C232;
            color: #FFFFFF;
            font-family: system-ui;
            font-size: 20px;
            font-weight: bold;
            margin-left: 30px;
            margin-top: 20px;
            width: 140px;
        }
        input {
            color: #222629;
            font-family: system-ui;
            font-size: 20px;
            margin-left: 10px;
            margin-top: 20px;
            width: 100px;
        }
    </style>
    <script>
        // callAPI function that takes the base and exponent numbers as parameters
        var callAPI = (base, exponent) => {
            // instantiate a headers object
            var myHeaders = new Headers();
            // add content type header to object
            myHeaders.append("Content-Type", "application/json");
            // using built in JSON utility package turn object to string and store in a variable
            var raw = JSON.stringify({"base": base, "exponent": exponent});
            // create a JSON object with parameters for API call and store in a variable
            var requestOptions = {
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: 'follow'
            };
            // make API call with parameters and use promises to get response
            fetch("YOUR API GATEWAY ENDPOINT", requestOptions)
            .then(response => response.text())
            .then(result => alert(JSON.parse(result).body))
            .catch(error => console.log('error', error));
        }
    </script>
</head>
<body>
    <h1>TO THE POWER OF MATH!</h1>
    <form>
        <label>Base number:</label>
        <input type="text" id="base">
        <label>...to the power of:</label>
        <input type="text" id="exponent">
        <!-- set button onClick method to call function we defined passing input values as parameters -->
        <button type="button" onclick="callAPI(document.getElementById('base').value, document.getElementById('exponent').value)">CALCULATE</button>
    </form>
</body>
</html>
```

# Liens :  DRIVE

### 1. **`PowerOfMathFunction - Lambda-ORIGINAL.txt`**
- **Fonction principale :** Calcule la puissance d'un nombre (base^exponent) et renvoie le résultat sous forme de JSON.
- **Usage :** Simple calcul sans stockage des résultats.

### 2. **`PowerOfMathFunction - Lambda-FINAL.txt`**
- **Fonction principale :** Calcule la puissance d'un nombre et stocke le résultat avec un horodatage dans DynamoDB, puis renvoie le résultat sous forme de JSON.
- **Usage :** Calcul avec enregistrement des résultats dans une base de données.
