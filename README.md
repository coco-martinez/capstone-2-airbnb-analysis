# Data-Driven Analysis of Airbnb Listings: Pricing, Demand & Location Recommendations

## Overview

This project analyzes Airbnb listing data across 31 US cities to understand which listing features are connected to pricing and demand.

The main placeholder for demand/bookings is `reviews_per_month` because actual booking data is not available. This is not a perfect measure of demand, but it gives a useful signal for comparing active listings.

The project also builds a location-based recommendation workflow. The goal is to enter a latitude, longitude, and radius, then return the top-performing listing types nearby, with pricing, demand, amenity, and model prediction outputs.

## Data Source

Data comes from Inside Airbnb: https://www.kaggle.com/datasets/konradb/inside-airbnb-usa

Raw detailed listing CSV files should be placed in:

`data/raw/`

Cleaned outputs are saved in:

`data/processed/`

## Project Structure

```text

capstone-2-airbnb-analysis/
├── data/
│   ├── raw/              # original CSV files, not tracked in git
│   └── processed/        # cleaned outputs, not tracked in git
├── models/               # saved model and feature list
├── notebooks/            # project notebooks
└── README.md

```

## Notebook Order

1. `01_data_wrangling.ipynb`  
   Cleans and combines the raw Airbnb files, standardizes amenities, fixes data types, and creates the first cleaned datasets.

2. `02_eda.ipynb`  
   Explores pricing, demand, location, room types, amenities, and market-level patterns.

3. `03_preprocessing_and_training.ipynb`  
   Prepares the data for modeling, selects features, handles train/test split, and saves modeling-ready data.

4. `04_modeling_v2.ipynb`  
   Tests baseline models and compares performance using regression metrics.

5. `05_product_features_and_segments.ipynb`  
   Creates product-style features like family-friendly, work-friendly, amenity strength, and pricing competitiveness scores.

6. `06_final_modeling_with_product_features.ipynb`  
   Builds and tunes the final Random Forest model. The model predicts `np.log1p(reviews_per_month)`.

7. `07_location_recommendation.ipynb`  
   Uses local comps and the final model to recommend listing types for a selected latitude, longitude, and radius. Notebook 07 depends on the final model and feature list saved in Notebook 06. To test a new location, update the location name, latitude, longitude, and radius in Notebook 07.

## Key Files

### Processed data

- `data/processed/airbnb_full.csv`
- `data/processed/airbnb_clean.csv`
- `data/processed/airbnb_product_modeling.csv`
- `data/processed/location_recommendation_output.csv`

### Saved model files

- `models/rf_location_recommendation_model.pkl`
- `models/rf_location_recommendation_features.pkl`

## Key Steps

- Combined Airbnb listings across multiple US cities.
- Standardized raw amenity strings into cleaner amenity categories.
- Corrected the demand dataset so missing or zero `reviews_per_month` values did not distort model training.
- Engineered product-style features for host trust, family friendliness, work friendliness, amenity strength, and pricing competitiveness.
- Compared baseline and tree-based regression models.
- Tuned a final Random Forest model.
- Built a reusable location recommendation workflow.

## Requirements

```bash
pip install pandas numpy scikit-learn matplotlib seaborn geopy joblib plotly
```

## Current Limitations

- reviews_per_month can't fully explain demand: a satisfied guest who stayed 3 nights, but didn't leave a review looks the same as a listing that didn't get booked. 
- Reviews could be positive or negative, so we can only infer based on review star ratings.
- reviews_per_month is a placeholder for demand since booking data isn't public. It does not show true bookings, occupancy, or revenue. Also, reviews_per_month isn't necessarily tied to a single booking, many guests can leave a review even if it's for the same trip.
- The model works best in markets represented in the training data. It should not be used as a reliable predictor for areas with little or no local data.
- Amenity themes like family-friendly, nature, and group-trip positioning aren't definitive and should be treated as directional.

## Future Improvements

- Add actual booking or occupancy data instead of relying on `reviews_per_month` as a placeholder for demand.
- Include listing age, since older listings may have more reviews because they have been active longer.
- Add seasonality data so the model can account for markets that perform differently during summer, holidays, or event seasons.
- Add more location-specific data, like distance to beaches, theme parks, convention centers, downtown areas, transit, and major attractions.
- Improve the amenity categories so the model can better separate standard amenities from ADR-supporting features.
- Add photo or computer vision features to capture design quality, views, room condition, and listing presentation.
- Use real revenue data, ADR, occupancy, or RevPAR if available, instead of only predicting demand.
- Build a user input tool where someone can enter a property address, listing details, host setup, and amenities to get a positioning recommendation.
