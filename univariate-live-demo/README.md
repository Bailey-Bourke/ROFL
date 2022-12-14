# Anomaly Detection Live Demo Instructions

![A screenshot of the live demo](image.webp)

Please note: this live demo is only intended to demonstrate the Anomaly Detector API on any CSV file that follows a simple schema. This demo loops over the provided data (with fake timestamps) to demonstrate the anomaly detection API. It does not use the real timestamps provided in the raw data, and should not be used in any production scenario. Once the demo is running, you will be able to see the raw data and the results from the anomaly detection API in the browser. Alternatively, you can also use the online Anomaly Detector [demo page](https://algoevaluation.azurewebsites.net/#/) to evaluate the API on your own data.

In your CSV file, you need at least two columns. A column for the timestamp and column for the values you want to perform anomaly detection on. In addition, you can include an *optional* dimension column that allows you to run the anomaly detection API across different categories (e.g. different sensors, different regions, or even different variables) through a drop down selection menu in the demo. The timestamp column should be in the ISO 8601 format `YYYY-MM-DDTHH:MM:SSZ`. If you intend to use your own CSV, please see the `sensor_data.csv` file for how to structure that CSV file.

## Step 1 - Setup

The demo should work in any Linux-based environment using any integrated development environment (IDE). If you choose to use Azure Machine Learning (AML) compute, you should install [VS Code](https://code.visualstudio.com/) locally to be able to run this demo. **If you are not using AML, just skip ahead to Step 2**.

With VS Code installed, navigate to your AML resource in the Azure portal, and click on `Launch Studio`. On the left, click on the `Compute` tab. Finally, under the `Applications` column, click the `VS Code` link corresponding to the compute instance that you would like to use. When asked by VS Code to `Allow an extension to open this URI?` click `Open`.

## Step 2 - Creating the virtual environment and installing the dependencies

Open a terminal in VS Code, and run the following command to create and activate a new conda virtual environment and install the dependencies:

```bash
conda create --name anomaly-detection-demo python=3.7
source activate anomaly-detection-demo
pip install -r requirements.txt
```

Next, enter the following two lines in your terminal, replacing `<anomaly-detector-api-key>` and `<anomaly-detector-endpoint>` with the values from your Anomaly Detector resource. You can find them by navigating to your Anomaly Detector resource in the Azure portal, and copying the Anomaly Detector key and endpoint from the "Keys and endpoints" section of the resource details.

```bash
export ANOMALY_DETECTOR_API_KEY=<anomaly-detector-api-key>
export ANOMALY_DETECTOR_ENDPOINT=<anomaly-detector-endpoint>
```

## Step 3 - Adjusting the configuration (Optional)

If you plan to use your own CSV to run this demo, take some time to review and adjust the default configuration in the `demo.py` file. For the purposes of this demo, the default configuration is stored within `demo.py` as a Python data class with default values. Feel free to modify the values to match your own needs.

```python 
    csv_name: str = "sensor_data.csv"  # Name of the csv file containing the data
    value_column: str = "sensor_readings"  # Name of the column containing the values
    timestamp_column: str = "timestamp"  # Name of the column containing the timestamps
    dimension_column: str = "sensor_name"  # (Optional) Name of the column containing a dimension (e.g. sensor name, or location, etc). If your data does not have this column, set it to None.
    window_size: int = 50  # Size of the window used to compute the anomaly score
    minute_resample: int = 5  # Resample the data to this minute resolution
    ad_mode: str = "entire"  # Anomaly detection mode to use. Can be "entire" for batch mode or "last" for last point mode.
```
If your data does not have the optional dimension column, please set the `dimension_column` in the `demo.py` script to `None`.

## Step 4 - Running the demo

Finally, navigate to this directory in your terminal. Make sure you have the `anomaly-detection-demo` virtual environment activated, then run the following command:

```bash
bokeh serve --port 5599 --show demo.py
```

A browser tab should open, and the demo should start running. Any detected anomaly will show in red. If the browser tab doesn't open, try to open it manually by navigating to `http://localhost:5599/demo` in the browser. If the demo does not start running, please double-check that you followed the steps above correctly. 

The demo will continue to run until you stop it. To stop the demo, close the browser tab and enter `Ctrl+C` in the terminal to stop the process.