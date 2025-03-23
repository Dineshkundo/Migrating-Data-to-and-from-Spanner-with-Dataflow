# Migrating-Data-to-and-from-Spanner-with-Dataflow

---

# CSV to Spanner Dataflow Pipeline

This project demonstrates how to use Apache Beam with Google Cloud Dataflow to load data from a CSV file into a Google Cloud Spanner database.

## Prerequisites

- Google Cloud Platform account
- A Google Cloud Project
- Enabled Google Cloud APIs:
  - Dataflow API
  - Spanner API
- Python 3.x environment

## Setup and Installation

### Step 1: Clone the repository

Clone the `training-data-analyst` repository to your local environment:

```bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

### Step 2: Navigate to the project folder

```bash
cd training-data-analyst/courses/understanding_spanner/dataflow
```

### Step 3: Create the Spanner database

Run the `create-spanner-pets-database.sh` script to create the Spanner database in the specified region:

```bash
bash ./create-spanner-pets-database.sh us-west1
```

### Step 4: Prepare the environment

Install the required Python packages using `pip`. Ensure you install `apache-beam[gcp]` for Dataflow support:

```bash
pip install apache-beam[gcp]==2.42.0
pip install apache-beam[dataframe]
```

### Step 5: Upload the CSV to Google Cloud Storage

Create a Google Cloud Storage bucket and upload the CSV file that contains pet data. This CSV file will be used as input for the pipeline.

```bash
gsutil mb -l us-west1 gs://$DEVSHELL_PROJECT_ID-data-flow
gsutil cp ./pets.csv gs://$DEVSHELL_PROJECT_ID-data-flow
```

### Step 6: Enable the necessary services

Ensure the necessary Google Cloud services (like Dataflow) are enabled:

```bash
gcloud services enable dataflow.googleapis.com
gcloud services enable spanner.googleapis.com
```

### Step 7: Run the Dataflow pipeline

Now, run the `csv-to-spanner.py` script using the Dataflow runner to process the CSV file and insert it into the Spanner database.

```bash
python csv-to-spanner.py \
    --region us-west1 \
    --worker_machine_type e2-standard-2 \
    --input gs://$DEVSHELL_PROJECT_ID-data-flow/pets.csv \
    --output gs://$DEVSHELL_PROJECT_ID-data-flow/results/outputs \
    --runner DataflowRunner \
    --project $DEVSHELL_PROJECT_ID \
    --temp_location gs://$DEVSHELL_PROJECT_ID-data-flow/tmp/
```

This will start the Dataflow job and load data from the CSV file into the Spanner database.

## Troubleshooting

- **Missing dependencies error:**
  If you encounter an error stating that the Dataflow runner is not available, install the `apache-beam[gcp]` package:

  ```bash
  pip install apache-beam[gcp]
  ```

- **ModuleNotFoundError (e.g., apitools)**:
  Ensure all dependencies are installed by running:

  ```bash
  pip install apache-beam[gcp]
  ```

## Conclusion

After completing the steps above, the pet data from the CSV file will be loaded into your Google Cloud Spanner database using Apache Beam and Dataflow. You can now use the Spanner database for querying and further processing.

---
