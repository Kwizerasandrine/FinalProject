# FinalProject

import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt

# Load the dataset
df = pd.read_csv(r"C:\Users\USER\synthetic_cybersecurity_dataset.csv")

# Display basic info
print("Shape of dataset:", df.shape)
print("\nColumn names:\n", df.columns.tolist())

# Preview the first few rows
df.head()

from sklearn.preprocessing import LabelEncoder

# Copy the dataframe to preserve original
df_encoded = df.copy()

# Create a LabelEncoder instance
le = LabelEncoder()

# Encode protocol and flag
df_encoded['protocol'] = le.fit_transform(df_encoded['protocol'])
df_encoded['flag'] = le.fit_transform(df_encoded['flag'])

# Encode the target label (attack types)
df_encoded['label_encoded'] = le.fit_transform(df_encoded['label'])

# Map encoded labels for reference
label_mapping = dict(zip(le.classes_, le.transform(le.classes_)))
print("Label Encoding Mapping:")
print(label_mapping)

# Drop original label if needed
# df_encoded.drop('label', axis=1, inplace=True)

# Preview the encoded dataset
df_encoded.head()


from sklearn.model_selection import train_test_split

# Separate features and target
X = df_encoded.drop(['label', 'label_encoded'], axis=1)
y = df_encoded['label_encoded']

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y)

print("Training set:", X_train.shape, "\nTest set:", X_test.shape)


from sklearn.model_selection import train_test_split

# Assuming `X_scaled_df` and `df_cleaned['label_encoded']` are ready
X_train, X_test, y_train, y_test = train_test_split(
    X_scaled_df, df_encoded['label_encoded'], 
    test_size=0.2, random_state=42, stratify=df_encoded['label_encoded']
)


# Add predictions back to test DataFrame
results_df = X_test.copy()
results_df['Actual_Label'] = y_test.values
results_df['Predicted_Label'] = y_pred

# Optional: map encoded labels back to names
inv_label_mapping = {v: k for k, v in label_mapping.items()}
results_df['Actual_Label_Name'] = results_df['Actual_Label'].map(inv_label_mapping)
results_df['Predicted_Label_Name'] = results_df['Predicted_Label'].map(inv_label_mapping)

# Save to CSV for Power BI
# Save to CSV for Power BI
import os

# Use current directory instead
results_file_path = "model_predictions_for_powerbi.csv"

# Alternative: create directory if you want to use the original path
# directory = "/mnt/data/"
# os.makedirs(directory, exist_ok=True)
# results_file_path = "/mnt/data/model_predictions_for_powerbi.csv"

results_df.to_csv(results_file_path, index=False)
print(f"File saved successfully as: {results_file_path}")

results_file_path

