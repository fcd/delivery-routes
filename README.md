# Delivery Routes

This program is used to calculate delivery routes given an origin (the materials depot from which all delivery trucks will leave), the number and types of trucks, and destinations. I wrote this for Herndon, VA's Troop 913 annual mulch fundraiser where we typically must deliver 6,000+ bags of mulch in a single (crazy) day.

## Requirements

* Python 2.7
* Pip
* VirtualEnv

## Setup

Source your virtual environment and then run ```pip install -r venv_requirements.txt``` to pull down all of the dependencies.

## Running

The program requires a CSV as input with all of the addresses along with a configuration file, outlined below.

The following command will output a routes.json file which should be used as input to the next stage:

```shell
$ ./router.py -c config.json <orders.csv>
```

The following command will output one PDF file per route, based on the input ```routes.json``` file:

```shell
$ ./printer.py -c config.json output/routes.json
```

### Configuration

The configuration file is a JSON file with the following fields:

```json
{
  "api_key": "Get it from developers.google.com",
  "trucks": [
    {
      "type": "Box Truck",
      "capacity": 135
    },
    {
      "type": "26 Foot Flat Truck",
      "capacity": 315
    }
  ],
  "contact": "123-456-7890 (John Smith)",
  "output_dir": "output",
  "origin": [38.950633, -77.397684]
}
```

The fields are explained below:
* api_key: This is the Google API key used to geocode addresses and map routes.
* trucks: This is the dictionary of trucks along with their capacity (in this case number of bags of mulch).
* contact: The contact information printed on the bottom of each delivery route (in case drivers need assistance).
* output_dir: The directory to write out all of the PDF files representing delivery routes.
* origin: The coordinates of the depot that all trucks start out from.

## Overview

The algorithm used is extremely simple and doesn't always generate the best road-routes as it simply compares geographic coordinates (lat, lon). It also assumes that all of the items to deliver are uniform in size/weight (which is true for bags of mulch). Thus the capcity of the trucks and the orders is uniform--more work would be required if you wanted this to calculate delivery of differing items. 

This could be improved by using Google's routing API but to do so would require many different route computations which would exceed the free tier of usage, thus I haven't bothered. Instead, to improve the routes, I output an intermediate file, ```routes.json```, which can be hand-edited to improve routes and then passed in to the program to print out the PDF routes.
