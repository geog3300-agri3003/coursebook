# Prompt engineering

The quality of an LLM’s response depends on the quality of the input. Prompt engineering is the process of constructing prompts for LLMs to return best possible answers. A prompt is the input you provide to LLM, often it is a question or an instruction. 

There are a range of techniques that you can use to improve the quality of AI generated responses. You can use more than one of these techniques in a single prompt. 

Beyond improving the quality of generated AI responses for a specific task, using prompt engineering techniques can improve your learning in general. For example, you can use role-based prompting and ask the LLM to suggest improvements to your Python script from the view of a senior developer or your GIS workflow from the view of senior geospatial analysis. By doing this, you will pick up pointers to improve a range of skills from programming, data analysis to clarity of writing. With chain-of-thought prompting you can see the steps the model goes through when solving a problem. Seeing this in action can help you refine your problem solving skills, see examples of how to deconstruct complex problems into discrete steps and pick up tips that you can transfer to different tasks. 

## TL;DR - Prompt template

Effective prompts provide explicit instructions and define the desired output format, give the LLM a role or relevant contextual information and encourage the LLM to think by providing a step-by-step account of its reasoning. It's also good practice to provide the LLM an example (if you have one) and iterate with the LLM to refine the solution. 

An example prompt template:
```
From the perspective of [INSERT ROLE HERE], generate [PROVIDE INSTRUCTIONS HERE]. This response is intended for [TARGET AUDIENCE / USER].

Generate the response using the following format [DEFINE OUTPUT FORMAT / STRUCTURE] using this example: [PROVIDE EXAMPLE, IF AVAILABLE].

Provide a step-by-step explanation of each step with a justification
```

## Explicit instructions and specificity

The place to start with crafting effective prompts is providing clear and specific instructions to the LLM. Be very clear on what kind of information you want generated. If you provide a general instruction or ask a vague question, you will likely get a general response. 

There are various ways you can improve the specificity of a prompt such as **providing contextual information** and **defining the output format**. 

When providing contextual informaton, you can you can specify if are you using the response to: better understand a topic? seek suggestions for improving scientific writing? improve the clarity of text for a policy audience? generate interactive visualisations for public viewing online? generate cartographic quality maps for a report? or improve the robustness of your code or GIS workflows? Provide this information in your prompt and the response will be tailored accordingly. 

Things to consider when defining the format of the response are: what language do you want code examples to be provided in (e.g. R, Python, JavaScript)? do you want generated text with references? what structure do you want the generated text to have (you can provide headings and sub-headings for the response in your prompt)?

Consider this prompt. It outlines the instruction for the LLM and provides a little ancillary information indicating that analysis will be performed in QGIS. However, it is left quite open as to the format and level of detail the LLM should generate in response.

!!! note "Prompt"
    Can you outline a methodology to compute the area of vegetation on bare earth roaded catchments, that drain into farm dams, from UAV images using QGIS?

    ??? note "Response (Gemini 2.0 Flash)"
        Here's a methodology to compute the area of vegetation on bare earth roaded catchments draining into farm dams using UAV images and QGIS:

        Data Acquisition and Preprocessing:

        * UAV Image Acquisition: Capture high-resolution UAV images of the catchment area. Ensure sufficient overlap (at least 70%) for accurate orthomosaic creation. Consider capturing images at different times of the day for varying light conditions if necessary. If possible, acquire multispectral imagery to aid in vegetation classification.
        * Orthomosaic and DEM Creation: Use photogrammetry software (e.g., Agisoft Metashape, Pix4D) to process the UAV images and generate an orthomosaic (georeferenced image) and a Digital * Elevation Model (DEM). Accurate georeferencing is crucial. Use Ground Control Points (GCPs) if possible for increased accuracy.
        * Boundary Delineation:
            * Catchment Boundary: Delineate the catchment boundary that contributes to the farm dam(s). Use the DEM to identify flow paths and ridgelines. This can be done manually in QGIS or using hydrological tools.
            * Farm Dam Boundary: Digitize the boundary of the farm dam(s) on the orthomosaic.
        * Road Network: Digitize the road network within the catchment. This will be used to identify areas of bare earth and potentially filter out vegetation growing on the road itself (if you only want vegetation off the roads).
        * Data Projection: Ensure all your data (orthomosaic, DEM, boundaries) are in the same projected coordinate system. This is essential for accurate area calculations.
        
        Vegetation Classification:

        * Image Enhancement (Optional): If needed, adjust the brightness, contrast, or perform other image enhancements to improve the visual distinction between vegetation and bare earth.
        * Vegetation Index Calculation (If using multispectral imagery): Calculate vegetation indices like NDVI (Normalized Difference Vegetation Index) or other relevant indices. These indices enhance the spectral signature of vegetation, making classification easier and more accurate. In QGIS, use the Raster Calculator for this.
        
        Supervised or Unsupervised Classification:
        
        * Supervised Classification: Create training areas (polygons) representing different land cover classes (e.g., vegetation, bare earth, water, roads). Use a classification algorithm (e.g., Random Forest, Support Vector Machine) in QGIS (e.g., Semi-Automatic Classification Plugin (SCP), Orfeo Toolbox (OTB)) to classify the orthomosaic based on these training areas. This is generally more accurate.
        * Unsupervised Classification: Use a clustering algorithm (e.g., K-means) to automatically group pixels into clusters based on their spectral characteristics. Then, manually assign these clusters to land cover classes. This is faster but less accurate.
        * Accuracy Assessment: Assess the accuracy of your classification by comparing the classified image to ground truth data (if available) or by visual inspection. You can create a confusion matrix to quantify the accuracy. Refine your training areas or classification parameters if necessary.
        
        Area Calculation:

        * Vectorization of Classified Raster: Convert the classified raster image (containing vegetation and bare earth classes) into a vector layer (polygons). QGIS's "Polygonize" tool can be used for this.
        * Area Calculation: Use the field calculator in the attribute table of the vectorized layer to calculate the area of each polygon. Filter the polygons to select only those classified as "vegetation." Sum the areas of these polygons to get the total vegetated area within the catchment.
        * Area within Catchment Boundary: Use the "Clip" tool in QGIS to clip the vectorized vegetation layer to the catchment boundary. This ensures you're only calculating the vegetated area within the defined catchment.
        * Area excluding roads (Optional): Use the "Difference" tool to subtract the road network polygons from the vegetation polygons. This will give you the vegetated area off the roads.
        
        Reporting and Visualization:

        * Summarize Results: Calculate the percentage of vegetated area within the catchment.
        * Create Maps: Generate maps showing the catchment boundary, farm dam, road network, and the classified vegetation. Use appropriate symbology and labels for clear communication.
        
        Software and Plugins:

        * QGIS: Free and open-source Geographic Information System.
        * Agisoft Metashape or Pix4D: Photogrammetry software for orthomosaic and DEM creation.
        * Semi-Automatic Classification Plugin (SCP) or Orfeo Toolbox (OTB) for QGIS: For image classification.
        
        Key Considerations:

        * Accuracy of UAV Data: The accuracy of your area calculation depends heavily on the quality of your UAV data (orthomosaic, DEM). GCPs are highly recommended.
        * Classification Accuracy: Careful selection of training areas (for supervised classification) or cluster interpretation (for unsupervised classification) is crucial for accurate results.
        * Resolution of UAV Imagery: Higher resolution imagery will generally provide more accurate results, especially for smaller vegetation patches.
        * Shadows: Shadows can affect vegetation classification. Consider capturing images when shadows are minimal.
        * Seasonal Variations: Vegetation cover can change seasonally. Be mindful of the timing of your UAV image acquisition.
        
        This methodology provides a comprehensive framework.  You may need to adapt it depending on the specifics of your study area and the available data. Remember to document all steps of your process for reproducibility.

We can improve this prompt in a couple of ways. For example, we could provide more information on what roaded catchments are indicating that they're human constructions and may not be easily delineated via DEM-based GIS catchment mapping tools. We could also provide more details on how we'd like to map vegetation (e.g. using the multispectral images to create a NDVI layer and setting threshold for vegetation cover). We can also provide more detail about the output format, such explicitly asking for detailed instructions for using QGIS. 

!!! note "Prompt"
    Can you outline a methodology to compute the area of vegetation on bare earth roaded catchments, that drain into farm dams, from UAV images using QGIS? 

    To compute the area of vegetation you will need to compute the NDVI from the UAV image and use a pixel NDVI threshold of greater than 0.2 to define vegetation. Provide detailed instructions and explanations for each step in the workflow including listing which QGIS operations and tools to use and where to find these tools in the QGIS user interface.

    ??? note "Response (Gemini 2.0 Flash)"
        Here's a detailed methodology to compute the area of vegetation using NDVI and a threshold in QGIS, specifically for bare earth roaded catchments draining into farm dams:

        Data Acquisition and Preprocessing (Same as before, but with emphasis on multispectral):

        * UAV Image Acquisition: Crucially, you must acquire multispectral imagery (at least Red, Green, and Near-Infrared bands). This is essential for calculating NDVI.
        * Orthomosaic and DEM Creation: Use photogrammetry software (e.g., Agisoft Metashape, Pix4D) to generate an orthomosaic and DEM. Ensure accurate georeferencing using GCPs if possible.
        * Boundary Delineation: Delineate the catchment boundary, farm dam(s), and road network as polygons in QGIS, using the DEM and orthomosaic as a guide.
        * Data Projection: Ensure all data are in the same projected coordinate system.
        
        NDVI Calculation in QGIS:

        * Open Raster Calculator: Go to Raster -> Raster calculator.
        * Enter NDVI Formula: In the Raster calculator expression box, enter the following formula, adjusting band names if necessary (e.g., if your Red band is not called "red"):
        `( "NIR" - "Red" ) / ( "NIR" + "Red" )`
        * Replace "NIR" and "Red" with the actual band names from your orthomosaic. You can see the band names by clicking the "..." button next to the expression box and expanding your raster layer.
        * Output Layer: Specify a name and location for the output NDVI raster file.
        * Click OK: QGIS will calculate the NDVI for each pixel. NDVI values typically range from -1 to +1, with higher values indicating more vegetation.
        
        Vegetation Classification using NDVI Threshold:

        * Reclassify Raster: Go to Raster -> Raster calculator.
        * Create Reclassification Expression: In the Raster calculator, use a conditional expression to classify pixels based on the NDVI threshold. For a threshold of 0.2:
        CASE WHEN "NDVI@1" > 0.2 THEN 1 ELSE 0 END
        * Replace "NDVI@1" with the name of your NDVI raster layer. 1 represents vegetation, and 0 represents non-vegetation (bare earth, water, etc.).
        * Output Layer: Specify a name and location for the reclassified raster (e.g., "vegetation_mask").
        * Click OK: QGIS will create a new raster where pixels with NDVI > 0.2 are assigned the value 1 (vegetation), and others are assigned 0.
        
        Refining the Vegetation Mask (Optional but Recommended):

        * Remove Small Patches (Sieve): Small, isolated pixels classified as vegetation might be noise. Go to Raster -> Analysis -> Sieve. Input your reclassified raster. Set the "Pixel threshold" to a value that removes small patches (e.g., 4 for removing patches smaller than 4 pixels).
        * Fill Holes: Small holes within larger vegetation patches can be filled. Go to Raster -> Analysis -> Fill nodata.
        
        Area Calculation:

        * Polygonize Raster: Convert the classified raster to a vector layer (polygons). Go to Raster -> Conversion -> Polygonize (raster to vector). Input your reclassified raster (or the sieved/filled raster if you performed those steps).
        * Calculate Area: Open the attribute table of the newly created polygon layer (right-click the layer -> Open Attribute Table).
        * Add Area Field: If an area field doesn't already exist, create one. Click the "Toggle Editing" button (pencil icon), then click the "Open Field Calculator" button (abacus icon).
        * New field name: area
        * New field type: Decimal number (real)
        * Expression: $area (This will calculate the area in the layer's units)
        * Click OK.
        * Filter Vegetation Polygons: In the attribute table, use the "Select by expression" tool (epsilon icon) to select polygons where the value of the reclassified field (usually named something like "DN") is equal to 1 (vegetation). The expression would be: "DN" = 1
        * Calculate Sum of Area: With the vegetation polygons selected, you can see the sum of the area of the selected features in the attribute table (usually at the bottom).
        
        Clipping to Catchment Boundary:

        * Clip Vector Layer: Go to Vector -> Geoprocessing Tools -> Clip. Input your vegetation polygon layer, and the catchment boundary polygon as the "Clip layer." This will give you the vegetated area within the catchment. Repeat the area calculation from step 5 on this clipped layer.
        * Clip Vector Layer to exclude roads (Optional): Go to Vector -> Geoprocessing Tools -> Difference. Input your clipped vegetation polygon layer, and the road network as the "Overlay layer." This will give you the vegetated area off the roads. Repeat the area calculation from step 5 on this difference layer.
        
        Reporting and Visualization:

        * Summarize Results: Calculate the percentage of vegetated area.
        * Create Maps: Generate maps showing the catchment, dam, roads, and the classified vegetation.
        
        Key Considerations:

        * NDVI Threshold: The 0.2 threshold is a starting point. You might need to adjust it based on your specific area and vegetation types. Experiment with different thresholds and validate your results.
        * Shadows and Water: Shadows can affect NDVI. Water bodies often have low NDVI values, so they might be classified as non-vegetation. You might need to use additional techniques (e.g., masking) to deal with these issues.
        * Accuracy: The accuracy of your area calculation depends on the quality of your UAV data, the chosen NDVI threshold, and the accuracy of your boundary delineations.
        
        This detailed workflow should help you calculate the vegetated area within your catchments. Remember to document your steps and justify your choices (e.g., the NDVI threshold you used).


## Chain-of-thought prompting

Chain-of-thought prompting instructs the LLM to break solutions to questions or instructions down into a series of intermediate problems that are solved individually. This technique is particularly effective for large, complex or multi-step tasks and reasoning problems. Chain-of-thought prompting encourages the model to focus attention on individual parts of the problem, not skip reasoning steps and often returns more accurate responses. 

You can implement chain-of-thought prompting via providing explicit instructions of the steps you want the model to work through (example 1) or finish your prompt with an instruction for the model to implement step-by-step reasoning (example 2). 

**Example 1:** 

!!! note "Prompt - example 1"
    You are a remote sensing and machine learning specialist and consultant. Your task is to develop a land cover classification product for a regional local government organisation in Western Australia. 

    You need to develop a land cover classification workflow that will generate a 10 m spatial resolution land cover map that includes the following classes: urban, water, grassland, scrubland, agriculture, mangrove, mudflats, open trees, forest. Please can you generate instructions and explanatory notes for the following steps:

    1. Generate training data that captures the geographic diversity of the land cover classes in the study area.
    2. Generate test data from a probability sample to support robust map accuracy assessment. 
    3. Download Sentinel-2 images from Planetary Computer.
    4. Combine data from Sentinel-2 images with the labelled training and test data and transform the data to a format suitable for machine learning model development. 
    5. Train a machine learning model to classify Sentinel-2 images into land cover maps. 
    6. Evaluate the model using the test data. 
    7. Generate a web map viewer for the classified land cover map land cover map. 

    Include Python code for each step where relevant. 

    ??? note "Response (ChatGPT)"
        Developing a 10-meter spatial resolution land cover map for a regional local government organization in Western Australia involves several key steps. Below is a detailed workflow, including Python code snippets where applicable.

        Generate Training Data

        Objective: Collect representative samples for each land cover class to train the classification model.

        * Field Surveys: Conduct on-site visits to collect GPS coordinates and photographs of distinct land cover types.
        * High-Resolution Imagery: Utilize platforms like Google Earth or commercial satellite imagery to visually identify and digitize polygons corresponding to each class.
        * Existing Datasets: Incorporate data from local environmental agencies or previous studies that have classified land cover in the region.
        * Note: Ensure that the training data captures the geographic diversity and variability of each class within the study area.

        Generate Test Data

        Objective: Create a statistically robust dataset to evaluate the accuracy of the land cover map.

        * Probability Sampling: Implement a stratified random sampling approach to select test points across different land cover classes.
        * Sample Size Determination: Calculate the number of samples needed per class to achieve a desired confidence level and margin of error.
        * Data Collection: For each selected point, record the true land cover class through field verification or high-resolution imagery interpretation.
        * Note: The test data should be independent of the training data to provide an unbiased assessment of model performance.

        Download Sentinel-2 Images from Planetary Computer

        Objective: Acquire Sentinel-2 imagery covering the study area for the relevant time period.

        * Utilize the sentinelsat Python package to search and download Sentinel-2 data from the Copernicus Open Access Hub.

        ```python
        from sentinelsat import SentinelAPI, read_geojson, geojson_to_wkt
        from datetime import date

        # User credentials for Copernicus Open Access Hub
        api = SentinelAPI('username', 'password', 'https://scihub.copernicus.eu/dhus')

        # Define the area of interest (AOI) by loading a geojson file
        footprint = geojson_to_wkt(read_geojson('aoi.geojson'))

        # Search for Sentinel-2 products over the AOI within the specified date range
        products = api.query(footprint,
                            date=('20220101', '20220131'),
                            platformname='Sentinel-2',
                            cloudcoverpercentage=(0, 30))

        # Download all results from the search
        api.download_all(products)
        ```

        Note: Replace 'username' and 'password' with your actual credentials. Adjust the date range and cloud cover percentage as needed.

        Prepare Data for Machine Learning

        Objective: Process and organize the imagery and labeled data for model training and evaluation.

        Preprocessing Sentinel-2 Imagery:
        
        * Atmospheric Correction: Apply correction algorithms (e.g., Sen2Cor) to convert Top-Of-Atmosphere (TOA) reflectance to Bottom-Of-Atmosphere (BOA) reflectance.
        * Resampling: Standardize all bands to a 10-meter resolution.
        * Clipping: Trim the imagery to the study area's boundaries.
        
        Feature Extraction:
        
        * Spectral Indices: Compute indices such as NDVI (Normalized Difference Vegetation Index) to enhance class separability.
        
        ```python
        import rasterio
        from rasterio.plot import show
        import numpy as np

        # Open the red and NIR bands
        with rasterio.open('B04.tif') as red:
            red_band = red.read(1).astype('float32')
        with rasterio.open('B08.tif') as nir:
            nir_band = nir.read(1).astype('float32')

        # Calculate NDVI
        ndvi = (nir_band - red_band) / (nir_band + red_band)

        # Save NDVI to a new GeoTIFF
        ndvi_meta = red.meta
        ndvi_meta.update(dtype=rasterio.float32, count=1)
        with rasterio.open('ndvi.tif', 'w', **ndvi_meta) as dst:
            dst.write(ndvi, 1)
        ```
        
        Data Integration:
        
        * Label Assignment: Overlay training and test data points on the processed imagery to extract corresponding pixel values and assign class labels.
        * Dataset Creation: Organize the extracted features and labels into structured arrays or dataframes suitable for input into machine learning algorithms.
        
        Train a Machine Learning Model

        Objective: Develop a model capable of classifying land cover types based on the input features.

        * Model Selection: Choose an appropriate algorithm, such as Random Forest, Support Vector Machine, or a Convolutional Neural Network, depending on the complexity and size of the dataset.
        
        Training the Model:
        
        ```python
        from sklearn.ensemble import RandomForestClassifier
        from sklearn.model_selection import train_test_split
        from sklearn.metrics import accuracy_score

        # Assume X contains feature vectors and y contains corresponding labels
        X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.3, random_state=42)

        # Initialize and train the Random Forest classifier
        rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
        rf_model.fit(X_train, y_train)

        # Validate the model
        y_pred = rf_model.predict(X_val)
        accuracy = accuracy_score(y_val, y_pred)
        print(f'Validation Accuracy: {accuracy:.2f}')
        ```

        Note: Hyperparameter tuning and cross-validation are essential to optimize model performance.

        Evaluate the Model

        Objective: Assess the accuracy and reliability of the land cover classification.

        * Confusion Matrix: Generate a confusion matrix to visualize the performance across different classes.
        
        ```python
        from sklearn.metrics import confusion_matrix, classification_report
        import seaborn as sns
        import matplotlib.pyplot as plt

        # Compute confusion matrix
        cm = confusion_matrix(y_val, y)
        ```

!!! note "Prompt - example 2"
    You are a remote sensing and machine learning specialist and consultant. Your task is to develop a land cover classification product for a regional local government organisation in Western Australia. 

    You need to develop a land cover classification workflow that will generate a 10 m spatial resolution land cover map that includes the following classes: urban, water, grassland, scrubland, agriculture, mangrove, mudflats, open trees, forest. The task requires that you need to generate training data that captures the geographic diversity of the land cover classes in the study area and test data from a probability sample. You need to create a data processing pipeline to download Sentinel-2 images from Microsoft’s Planetary Computer and combine it with the labelled training and test data. You need to train a machine learning model to classify Sentinel-2 images into land cover maps and evaluate this model. The final task is to visualise the land cover map on a slippy web map. 

    Can you outline the solution in a step-by-step manner. For each step, please explain why this approach is suitable for this task. Also, include Python code for each step where relevant.

    ??? note "Response (Perplexity)"
        Certainly! I'll outline a step-by-step solution for developing a land cover classification product using Sentinel-2 imagery and machine learning techniques. This approach is tailored for a regional local government organization in Western Australia, aiming to create a 10 m spatial resolution land cover map with specific classes.

        Step 1: Data Collection and Preparation

        Sentinel-2 Image Acquisition

        We'll use Microsoft's Planetary Computer to access Sentinel-2 imagery. This platform provides easy access to a vast archive of Earth observation data, including Sentinel-2 images[^1].

        ```python
        import planetary_computer
        import pystac_client
        import rasterio
        import numpy as np

        # Initialize the Planetary Computer STAC API client
        catalog = pystac_client.Client.open("https://planetarycomputer.microsoft.com/api/stac/v1")

        # Search for Sentinel-2 imagery
        search = catalog.search(
            collections=["sentinel-2-l2a"],
            bbox=[115.0, -35.0, 129.0, -13.0],  # Approximate bounding box for Western Australia
            datetime="2024-01-01/2025-02-23",
            query={"eo:cloud_cover": {"lt": 20}}
        )

        # Download and stack relevant bands
        items = list(search.get_items())
        for item in items:
            signed_item = planetary_computer.sign(item)
            bands = ["B02", "B03", "B04", "B08", "B11", "B12"]  # Blue, Green, Red, NIR, SWIR1, SWIR2
            arrays = []
            for band in bands:
                with rasterio.open(signed_item.assets[band].href) as src:
                    arrays.append(src.read(1))
            stacked_image = np.stack(arrays)
        ```

        This approach is suitable because it allows us to efficiently access and process large volumes of satellite imagery without the need for local storage[^1].

        Training Data Generation

        To capture the geographic diversity of land cover classes, we'll use a stratified random sampling approach:

        ```python
        import geopandas as gpd
        from sklearn.model_selection import train_test_split

        # Load a shapefile of the study area
        study_area = gpd.read_file("study_area.shp")

        # Generate stratified random points
        points = study_area.sample(n=1000, stratify="land_cover_class")

        # Split into training and validation sets
        train_points, val_points = train_test_split(points, test_size=0.2, stratify=points["land_cover_class"])

        # Save points for field verification
        train_points.to_file("training_points.shp")
        val_points.to_file("validation_points.shp")
        ```

        This method ensures that we have representative samples for each land cover class across the study area[^9].

        Step 2: Image Processing and Feature Extraction

        We'll create spectral indices and texture features to enhance the classification:

        ```python
        def calculate_indices(image):
            blue, green, red, nir, swir1, swir2 = image

            ndvi = (nir - red) / (nir + red)
            ndwi = (green - nir) / (green + nir)
            savi = 1.5 * (nir - red) / (nir + red + 0.5)

            return np.vstack((image, ndvi, ndwi, savi))

        processed_images = [calculate_indices(img) for img in stacked_images]
        ```

        These spectral indices are particularly useful for distinguishing between vegetation types, water bodies, and built-up areas[^4].

        Step 3: Machine Learning Model Training

        We'll use a Random Forest classifier, which has shown good performance in land cover classification tasks[^4]:

        ```python
        from sklearn.ensemble import RandomForestClassifier
        from sklearn.model_selection import GridSearchCV

        # Prepare training data
        X = extract_features(train_points, processed_images)
        y = train_points["land_cover_class"]

        # Define parameter grid
        param_grid = {
            'n_estimators': [100, 200, 300],
            'max_depth': [10, 20, 30, None],
            'min_samples_split': [2, 5, 10]
        }

        # Perform grid search
        rf = RandomForestClassifier(random_state=42)
        grid_search = GridSearchCV(rf, param_grid, cv=5, n_jobs=-1)
        grid_search.fit(X, y)

        # Train final model
        best_rf = grid_search.best_estimator_
        best_rf.fit(X, y)
        ```

        Random Forest is suitable for this task due to its ability to handle high-dimensional data, resistance to overfitting, and good performance on imbalanced datasets[^4].

        Step 4: Model Evaluation

        We'll use the validation set to assess the model's performance:

        ```python
        from sklearn.metrics import accuracy_score, cohen_kappa_score, classification_report

        # Prepare validation data
        X_val = extract_features(val_points, processed_images)
        y_val = val_points["land_cover_class"]

        # Make predictions
        y_pred = best_rf.predict(X_val)

        # Calculate metrics
        accuracy = accuracy_score(y_val, y_pred)
        kappa = cohen_kappa_score(y_val, y_pred)
        report = classification_report(y_val, y_pred)

        print(f"Accuracy: {accuracy}")
        print(f"Kappa: {kappa}")
        print(report)
        ```

        These metrics provide a comprehensive assessment of the model's performance, with the Kappa coefficient being particularly useful for multi-class problems[^9].

        Step 5: Land Cover Map Generation

        Apply the trained model to the entire study area:

        ```python
        def predict_landcover(image, model):
            features = calculate_indices(image)
            prediction = model.predict(features.reshape(-1, features.shape[^0]))
            return prediction.reshape(image.shape[^1], image.shape[^2])

        landcover_map = predict_landcover(stacked_image, best_rf)
        ```

        Step 6: Visualization on a Slippy Web Map

        We'll use Folium to create an interactive web map:

        ```python
        import folium
        from branca.colormap import LinearColormap

        # Create a color map
        colors = ['#FF0000', '#00FF00', '#0000FF', ...]  # Define colors for each class
        color_map = LinearColormap(colors=colors, vmin=0, vmax=len(colors))

        # Create a map centered on the study area
        m = folium.Map(location=[-27.0, 121.0], zoom_start=6)

        # Add the land cover layer
        folium.raster_layers.ImageOverlay(
            landcover_map,
            bounds=[[study_area.total_bounds[^1], study_area.total_bounds[^0]],
                    [study_area.total_bounds[^3], study_area.total_bounds[^2]]],
            colormap=color_map,
            opacity=0.7
        ).add_to(m)

        # Add a legend
        color_map.add_to(m)

        # Save the map
        m.save("landcover_map.html")
        ```

        This interactive map allows users to explore the land cover classification results easily[^5].

        This comprehensive workflow combines state-of-the-art satellite imagery, robust machine learning techniques, and interactive visualization to create an accurate and user-friendly land cover classification product for the Western Australian local government organization. The use of Sentinel-2 imagery provides high spatial resolution (10 m) suitable for detailed land cover mapping, while the Random Forest classifier offers good performance and interpretability. The final web map visualization ensures that the results are accessible and easy to understand for decision-makers and stakeholders.

        [^1]: https://knowledge.dea.ga.gov.au/data/product/dea-land-cover-landsat/index.html

        [^2]: https://www.luxcarta.com/blog/gis/land-use-classification

        [^3]: https://www.mdpi.com/2073-445X/11/7/993

        [^4]: https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2022.964279/full

        [^5]: https://www.esri.com/arcgis-blog/products/arcgis/imagery/pretrained-land-cover-models/

        [^6]: https://knowledge.dea.ga.gov.au/data/product/ga-land-cover-terra-modis/index.html

        [^7]: https://www.youtube.com/watch?v=DHj7C3ovBkM

        [^8]: https://www.nature.com/articles/s41597-022-01775-8

        [^9]: https://isprs-archives.copernicus.org/articles/XLVIII-4-2024/263/2024/isprs-archives-XLVIII-4-2024-263-2024.pdf

        [^10]: https://www.tern.org.au/wp-content/uploads/TERN_CalVal_Handbook_web.pdf

        [^11]: https://www.mathworks.com/help/images/classify-land-cover-using-sentinel-2-multispectral-images.html

        [^12]: https://ceur-ws.org/Vol-3699/paper8.pdf

        [^13]: https://pure.iiasa.ac.at/18924/1/202301_SPACES-Remote-Sensing-1.pdf

        [^14]: https://doc.arcgis.com/en/pretrained-models/latest/imagery/using-land-cover-classification-sentinel-2-.htm


## Role-based prompting

Role-based prompting assigns the LLM a “role” for to the task or interaction and it can improve the accuracy of responses and their relevance for target audiences or tasks. For example, if you want the LLM to generate detailed content for a technical or scientific topic, you could assign it the role of a domain expert such as a “remote sensing specialist” or “senior geospatial analyst”. Or, if you are looking for clear and accessible explanation of a concept you could assign the LLM the role of a “teacher”. 

!!! note "Prompt"
    You are a senior geospatial data scientist. Can you make suggestions for improving this workflow for crop type classification using remote sensing data?

    1. Generate crop type labels using Google Street View
    2. Generate predictor variables using single-date Landsat data
    3. Train machine learning model to associate Landsat data with crop type labels
    4. Evaluate model using government datasets and farmer records

    ??? note "Response (Perplexity with ChatGPT's o3-mini reasoning model)"
        To improve the workflow for crop type classification using remote sensing data, I suggest the following enhancements based on the current workflow and relevant findings from the search results:

        **Use Multi-Date Satellite Imagery Instead of Single-Date Landsat Data**

        **Reason:** Single-date imagery may not capture the full phenological cycle of crops, which is crucial for distinguishing between crop types. Studies show that multi-date imagery improves classification accuracy by leveraging spectral and temporal variations across different stages of crop growth.

        **Improvement:**

        - Incorporate multi-date Landsat or Sentinel-2 data to capture key phenological stages (e.g., planting, peak growth, harvest).
        - Generate vegetation indices (e.g., NDVI, EVI) for each date to enhance feature space.

        **Code Example:**

        ```python
        from datetime import datetime
        import planetary_computer

        # Define time periods for multi-date imagery
        time_periods = ["2024-05-01/2024-06-30", "2024-08-01/2024-09-30", "2024-11-01/2024-12-31"]

        # Search for multi-date Landsat data
        images = []
        for period in time_periods:
            search = catalog.search(
                collections=["landsat-c2-l2"],
                bbox=[115.0, -35.0, 129.0, -13.0],  # Western Australia bounding box
                datetime=period,
                query={"eo:cloud_cover": {"lt": 20}}
            )
            images.extend(list(search.get_items()))
        ```

        **Enhance Crop Type Labels with Automated Deep Learning on Street View Imagery**

        **Reason:** Generating crop type labels manually from Google Street View (GSV) is time-consuming and prone to errors. Automated deep learning pipelines can efficiently extract crop type information from street-level imagery with minimal manual labeling.

        **Improvement:**

        - Use a Convolutional Neural Network (CNN) or pre-trained models (e.g., GPT-4V) to classify GSV images automatically.
        - Combine weakly labeled GSV data with remote sensing time series to generate robust training datasets.

        **Code Example:**

        ```python
        from tensorflow.keras.models import load_model

        # Load pre-trained CNN model for crop type classification
        model = load_model("crop_type_cnn.h5")

        # Predict crop types from GSV images
        def predict_crop_type(image_path):
            image = preprocess_image(image_path)  # Preprocess image for CNN input
            prediction = model.predict(image)
            return np.argmax(prediction)

        # Apply predictions to a dataset of GSV images
        crop_labels = [predict_crop_type(img_path) for img_path in gsv_image_paths]
        ```

        Integrate Phenology-Based Approaches**

        **Reason:** Crop-specific phenological patterns (e.g., growth cycles) can help distinguish between similar crops. Incorporating phenology-based classification methods enhances accuracy by leveraging temporal changes in vegetation indices.

        **Improvement:**

        - Use time-series analysis of vegetation indices like NDVI, SAVI, and EVI to identify unique crop phenologies.
        - Combine spectral indices with texture features for improved discrimination.

        **Code Example:**

        ```python
        import numpy as np

        # Calculate NDVI over multiple dates
        def calculate_ndvi(nir_band, red_band):
            return (nir_band - red_band) / (nir_band + red_band)

        ndvi_series = [calculate_ndvi(images[i]["B5"], images[i]["B4"]) for i in range(len(images))]
        phenology_features = np.stack(ndvi_series, axis=-1)
        ```

        **Leverage Sentinel-2 Data for Higher Spatial Resolution**

        **Reason:** Sentinel-2 provides 10 m spatial resolution compared to Landsat's 30 m, which is more suitable for smallholder farms or heterogeneous landscapes often found in regions like Western Australia.

        **Improvement:**

        - Replace Landsat data with Sentinel-2 imagery to capture finer spatial details.
        - Use Sentinel-2's additional bands (e.g., Red Edge) to improve spectral discrimination between crops.

        Evaluate Model with Independent Datasets and Stratified Sampling**

        **Reason:** Current evaluation relies on government datasets and farmer records, which may not be spatially representative or fully accurate. Stratified sampling ensures better geographic and class representation.

        **Improvement:**

        - Use stratified random sampling to create validation datasets covering diverse land cover types.
        - Supplement evaluation with independent datasets like the Cropland Data Layer (CDL) or other open-access crop maps.

        **Code Example:**

        ```python
        from sklearn.model_selection import train_test_split

        # Stratify by crop type class
        train_data, test_data = train_test_split(data, test_size=0.2, stratify=data["crop_type"])
        ```

        **Incorporate Ensemble Learning Models**

        **Reason:** Ensemble methods such as Random Forests or Gradient Boosting outperform single models by combining predictions from multiple learners.

        **Improvement:**

        - Train ensemble models on multi-date features and phenology-based indices.
        - Evaluate ensemble performance against single classifiers.

        **Code Example:**

        ```python
        from sklearn.ensemble import RandomForestClassifier

        # Train Random Forest model
        rf_model = RandomForestClassifier(n_estimators=100)
        rf_model.fit(X_train, y_train)

        # Predict and evaluate
        y_pred = rf_model.predict(X_test)
        ```

        **Visualize Results Using Interactive Maps**

        **Reason:** Visualizing classification results on a web map improves stakeholder accessibility and decision-making.

        **Improvement:**

        - Use tools like Folium or Kepler.gl to create interactive maps showing classified crop types.
        - Include overlays of ground truth points and confidence scores.

        **Code Example:**

        ```python
        import folium

        # Create an interactive map with classified results
        m = folium.Map(location=[-27.0, 121.0], zoom_start=6)
        folium.raster_layers.ImageOverlay(
            image=classified_map,
            bounds=[[study_area.bounds[^1], study_area.bounds[^0]],
                    [study_area.bounds[^3], study_area.bounds[^2]]],
            opacity=0.6,
        ).add_to(m)
        m.save("crop_classification_map.html")
        ```

        Summary of Improvements:

        1. Use multi-date imagery to capture phenological variations.
        2. Automate label generation from GSV using deep learning.
        3. Incorporate phenology-based approaches for temporal analysis.
        4. Replace Landsat with Sentinel-2 for higher spatial resolution.
        5. Evaluate using stratified sampling and independent datasets.
        6. Train ensemble models for better predictive performance.
        7. Visualize results interactively for stakeholders.

        These enhancements address limitations in the current workflow by improving data quality, leveraging advanced machine learning techniques, and providing actionable insights through visualization tools.

        [^1]: https://arxiv.org/abs/2309.05930

        [^2]: https://arxiv.org/html/2309.05930v2

        [^3]: https://pure.iiasa.ac.at/19041/1/geographies-03-00029.pdf

        [^4]: https://www.frontiersin.org/journals/sustainable-food-systems/articles/10.3389/fsufs.2020.00099/full

        [^5]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9950293/

        [^6]: https://www.frontiersin.org/articles/10.3389/fenvs.2022.991173/full

        [^7]: https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2023.1196634/full

        [^8]: https://scholarworks.lib.csusb.edu/cgi/viewcontent.cgi?article=3191\&context=etd

        [^9]: https://docs.openeo.cloud/usecases/crop-classification/

        [^10]: https://hdsr.mitpress.mit.edu/pub/ou89oggk

        [^11]: https://grdc.com.au/__data/assets/pdf_file/0018/572310/Paper-Potgieter-Andries-February-2022.pdf

        [^12]: https://onlinelibrary.wiley.com/doi/10.1155/2021/8810279

        [^13]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9693710/

        [^14]: http://www.agriculturejournal.org/volume11number3/crop-selection-and-yield-prediction-using-machine-learning-approach/

        [^15]: https://www.mdpi.com/2072-4292/13/14/2790

        [^16]: https://about.google/brand-resource-center/products-and-services/geo-guidelines/

        [^17]: https://www.researchgate.net/publication/330509635_Crop_type_mapping_without_field-level_labels_Random_forest_transfer_and_unsupervised_clustering_techniques

        [^18]: https://dl.acm.org/doi/10.1609/aaai.v38i20.30225

        [^19]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11014375/

        [^20]: https://www.researchgate.net/publication/347549848_Exploring_Google_Street_View_with_deep_learning_for_crop_type_mapping

        [^21]: https://www.tandfonline.com/doi/full/10.1080/01431161.2023.2205984

        [^22]: https://learn.geo4.dev/Satellite Crop Mapping.html

        [^23]: https://www.mdpi.com/2072-4292/15/13/3414

        [^24]: https://www.researchgate.net/publication/349189090_Identification_of_Crop_Type_in_Crowdsourced_Road_View_Photos_with_Deep_Convolutional_Neural_Network

        [^25]: https://openaccess.thecvf.com/content/CVPR2021W/EarthVision/papers/Tseng_Learning_To_Predict_Crop_Type_From_Heterogeneous_Sparse_Labels_Using_CVPRW_2021_paper.pdf

        [^26]: https://www.mdpi.com/2072-4292/13/5/911

        [^27]: https://www.tandfonline.com/doi/full/10.1080/22797254.2017.1401909

        [^28]: https://academic.oup.com/insilicoplants/article/3/1/diab017/6279904

        [^29]: https://pubs.usgs.gov/publication/ofr76603

        [^30]: https://www.asprs.org/wp-content/uploads/pers/1982journal/jan/1982_jan_111-119.pdf

        [^31]: https://isprs-archives.copernicus.org/articles/XLII-5/683/2018/isprs-archives-XLII-5-683-2018.pdf

        [^32]: https://www.mdpi.com/2072-4292/7/12/15820

        [^33]: https://academic.oup.com/bioscience/article/54/6/535/294251

        [^34]: https://eartharxiv.org/repository/object/197/download/394/

        [^35]: https://www.tandfonline.com/doi/full/10.1080/10106049.2024.2361733

        [^36]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9960281/

        [^37]: https://www.mdpi.com/2073-4395/9/6/309

        [^38]: https://academic.oup.com/nsr/article/10/4/nwac290/6939854

        [^39]: https://www.mdpi.com/2072-4292/5/10/4857

        [^40]: https://www.tandfonline.com/doi/full/10.1080/10095020.2022.2100287

        [^41]: https://ieeexplore.ieee.org/document/699586/

        [^42]: https://www.mdpi.com/2072-4292/16/9/1493

        [^43]: https://ieeexplore.ieee.org/document/10108000/

        [^44]: https://www.mdpi.com/2077-0472/13/5/965

        [^45]: https://ieeexplore.ieee.org/document/9441483/

        [^46]: https://www.researchgate.net/figure/The-proposed-ensemble-model-detailed-phases-for-crop-identification_fig3_367637760

        [^47]: https://ieeexplore.ieee.org/document/10032208

        [^48]: https://www.researchgate.net/publication/361257829_The_Classification_Method_Study_of_Crops_Remote_Sensing_with_Deep_Learning_Machine_Learning_and_Google_Earth_Engine

        [^49]: https://dl.acm.org/doi/10.1007/s11042-022-13946-1

        [^50]: https://www.researchgate.net/publication/382163751_Crop_classification_method_using_multifeature_optimal_selection_combined_with_an_ensemble_learning_model_a_case_study_of_the_Sanjiang_Plain

        [^51]: https://pmc.ncbi.nlm.nih.gov/articles/PMC10906854/

        [^52]: https://evaluation.treasury.gov.au/publications/government-administrative-data-sources-evaluation-australia

        [^53]: https://openknowledge.fao.org/server/api/core/bitstreams/ca36b8c1-99b6-4cad-ad88-ff018647a474/content

        [^54]: https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-foundation-model-evaluate-overview.html

        [^55]: https://www.researchgate.net/publication/372430884_Productive_Crop_Field_Detection_A_New_Dataset_and_Deep_Learning_Benchmark_Results

        [^56]: https://www.tandfonline.com/doi/full/10.1080/15481603.2024.2387393

        [^57]: https://www.infrastructure.gov.au/sites/default/files/documents/BCR Open government data report - final - 5 Feb2016.pdf

        [^58]: https://www.mdpi.com/1424-8220/23/4/1779

        [^59]: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-188.pdf

        [^60]: https://pmc.ncbi.nlm.nih.gov/articles/PMC7914883/

        [^61]: https://www.tandfonline.com/doi/full/10.1080/01431161.2023.2192881

        [^62]: https://news.mit.edu/2024/mit-researchers-remotely-map-crops-fields-0215

        [^63]: https://developers.google.com/earth-engine/tutorials/community/classify-maizeland-ng

        [^64]: https://www.reddit.com/r/degoogle/comments/157cklc/alternatives_to_google_streetview/

        [^65]: https://www.mdpi.com/2072-4292/7/10/13843

        [^66]: https://www.mdpi.com/2072-4292/7/8/10400

        [^67]: https://dida.do/projects/crop-type-classification

        [^68]: https://www.mdpi.com/2072-4292/16/1/127

        [^69]: https://pmc.ncbi.nlm.nih.gov/articles/PMC7615539/

        [^70]: https://www.mdpi.com/2072-4292/16/5/917

        [^71]: https://magnimindacademy.com/blog/building-a-rag-evaluation-dataset-a-step-by-step-guide-using-document-sources/


## Example-based prompting

LLMs can generate responses to questions or instructions without seeing any examples; this is called zero-shot prompting. However, you can provide one (single-shot prompting) or multiple (many-shot prompting) examples that illustrate the style of response you’d like returned or the logic the LLM should use to solve problems. 

!!! note "Prompt"
    Can you write Python functions to compute the following vegetation indices: EVI, OSAVI and NDWI? The functions should take in a Sentinel-2 image stored as a Numpy array. Use the following as an example format:

    ```python
    import numpy as np

    def calculate_ndvi(red, nir):
        """
        Calculates the Normalized Difference Vegetation Index (NDVI).

        Args:
            red (numpy.ndarray): Red band array.
            nir (numpy.ndarray): Near-infrared band array.

        Returns:
            numpy.ndarray: NDVI array.
        """
        
        # Prevent division by zero
        ndvi = np.where((nir + red) == 0, 0, (nir - red) / (nir + red))
        return ndvi
    ```

## Multi-media / multi-modal prompting

<a href="https://ai.google.dev/gemini-api/docs/file-prompting-strategies" target="_blank">Prompts to multi-modal models can include more than one type of data</a>. For example, you could upload a file, image or code alongside providing a written prompt. Be clear when prompting the model to use the all the data sources; if you upload an image you can start your prompt by asking the model to describe the image (focusing the model’s attention on the image) before generating the response you’re actually after. Multi-media prompting allows you to provide more context the model; this can improve the relevancy and factuality of generated responses. 

While it's not about agricultural or geospatial data, this is a great <a href="https://developers.googleblog.com/en/how-its-made-interacting-with-gemini-through-multimodal-prompting/" target="_blank">guide to multi-modal prompting</a>. However, this can illustrate ways you can use multi-modal prompting for spatial and agricultural analysis tasks. Here are some examples for you to try out:

* **Spatial reasoning and logic**: upload a map and prompt the AI to detect spatial patterns and interpret them (e.g. upload a map of crop yield within a paddock and prompt the AI to describe the spatial pattern of yield, comment on spatial variability in yield and identify underperforming zones).
* **Image sequences**: upload a "film strip" of images an prompt the AI to describe the spatio-temporal dynamics present in the images (e.g. upload a sequence of RGB satellite images of a canola field and ask the AI to describe the spatio-temporal pattern of flowering and explain the variability in timing and intensity of flowering within the field; upload regional maps of precipitation and prompt the AI to detect drying trends or drought risk; upload a series of land cover maps and prompt the AI to describe patterns in deforestation or agricultural expansion).
* **Image feedback**: upload an image (e.g. a map or figure) to the AI and ask for improvements for communicating key patterns, insights or information or ask for suggestions on how to edit for different audiences (e.g. a scientific graph to a graph for policy brief or infographic).
* **Video streaming and conversations**: With <a href="https://aistudio.google.com" target="_blank">Google’s AI studio</a> you can screenshare a paper you're reading, your coding environment or desktop GIS and have a verbal conversation about what's on your screen. For example, you could ask the AI to help explain a figure in a paper or report on your screen or help you navigate around QGIS to find operations. 
* **Presentation feedback**: Share your web cam with <a href="https://aistudio.google.com" target="_blank">Google’s AI studio</a> and practice a presentation and get feedback on ways to improve delivery. 

!!! note "Prompt"
    The uploaded image is a map of a land cover classification generated for a region in Western Australia from Sentinel-2 images processed by a machine learning model. Can you describe the land cover map? Can you make a series of suggestion for how the map can be improved? 


## Iterative prompting

Iterative prompting can be an effective technique to generate more accurate and relevant responses. Often, AI tools don't generate a correct or the most relevant prompt at the first attempt. In successive prompts you can provide feedback to the AI on how it should refine its responses. This has multiple benefits; with successive prompts you are providing more context to the AI tool, which it can use to improve the quality of the response, and you get the opportunity to clarify your thinking or develop new ideas as you reflect on the AI's responses.

Let's use the previous prompt example from the chain-of-thought prompting, which asks for a worfklow to generate a land cover map, to demonstrate an iterative prompting workflow. We'll focus on iteratively refining and adding detail to the part of this workflow that involves generating the training data. This is the initial prompt:

!!! note "Prompt"
    You are a remote sensing and machine learning specialist and consultant. Your task is to develop a land cover classification product for a regional local government organisation in Western Australia. 

    You need to develop a land cover classification workflow that will generate a 10 m spatial resolution land cover map that includes the following classes: urban, water, grassland, scrubland, agriculture, mangrove, mudflats, open trees, forest. Please can you generate instructions and explanatory notes for the following steps:

    1. Generate training data that captures the geographic diversity of the land cover classes in the study area.
    2. Generate test data from a probability sample to support robust map accuracy assessment. 
    3. Download Sentinel-2 images from Planetary Computer.
    4. Combine data from Sentinel-2 images with the labelled training and test data and transform the data to a format suitable for machine learning model development. 
    5. Train a machine learning model to classify Sentinel-2 images into land cover maps. 
    6. Evaluate the model using the test data. 
    7. Generate a web map viewer for the classified land cover map land cover map. 

    Include Python code for each step where relevant. 

    ??? note "Response (Gemini 2.0 Flash)"
        Step-by-Step Workflow:

        Generate Training Data

        Concept: Training data is crucial for supervised learning. It should be representative of the land cover classes across the study area.

        Method:

        * Visual Interpretation: Utilize high-resolution imagery (e.g., Google Earth, Nearmap) as a base layer.
        * Stratified Random Sampling: Divide the study area into zones based on known land cover patterns. Generate random points within each zone.
        * Manual Digitization: Digitize polygons around representative areas of each land cover class at the sampled points.
        * Geodatabase: Store the digitized polygons in a GeoJSON or shapefile format, with an attribute field indicating the land cover class.

        Considerations:

        * Ensure a balanced representation of each class.
        * Capture the variability within each class (e.g., different types of urban areas, varying densities of forest).
        * Use expert knowledge of the region to identify and delineate complex land cover types.

        Python (Example - Geopandas for creating a basic training file):

        ```python
        import geopandas as gpd
        from shapely.geometry import Polygon

        # Example polygons (replace with your actual digitized polygons)
        data = {
            'geometry': [
                Polygon([(115, -32), (115.1, -32), (115.1, -32.1), (115, -32.1)]),  # Urban
                Polygon([(116, -33), (116.1, -33), (116.1, -33.1), (116, -33.1)]),  # Forest
                #... more polygons
            ],
            'class': ['urban', 'forest']  # Corresponding land cover classes
        }

        gdf_train = gpd.GeoDataFrame(data, crs="EPSG:4326")  # Set CRS to WGS 84
        gdf_train.to_file("training_data.geojson", driver="GeoJSON")
        ```

It's an OK templare to start with, but the code snippet and instructions are incomplete or too vague to implement and we can add more detail to the explanatory notes. Let's go back to the AI and prompt it to provide more information:

!!! note "Prompt"
    Focusing on step 1 (generating training data) can you provide detailed instructions on how to implement stratified sampling to generate a series of points that are representative of the region? 

    Include information on the datasets required to do this, provide a Python code example, and reflect on any limitations with this approach with respect to training a land cover classifier.

    ??? note "Response (Gemini 2.0 Flash)"
        Data Requirements

        * Study Area Boundary: A polygon shapefile or GeoJSON defining the region of interest.
        * Stratification Layers: Raster or vector datasets that capture the geographic variability within your study area. These could include:
        * Elevation: A digital elevation model (DEM) to capture variations in terrain.
        * Landforms: A landform classification map to differentiate between plains, hills, valleys, etc.
        * Soil Types: A soil map to account for differences in soil properties.
        * Vegetation Indices: Pre-calculated NDVI or other vegetation indices from existing imagery.
        * Existing Land Cover: A coarse-resolution land cover map to broadly stratify the area.

        Stratification Process

        * Combine Layers (Optional): If using multiple stratification layers, you might need to combine them into a single composite layer. This can be done using raster algebra or spatial joins, depending on the data format.
        * Define Strata: Divide the composite layer into distinct classes or zones. The number and type of strata will depend on the complexity of your study area and the available data. For example, you could combine elevation and landform to create strata like "lowland plains," "upland hills," etc.
        * Allocate Sample Points: Determine the desired number of sample points for each stratum. This can be done proportionally to the area of each stratum or based on prior knowledge of land cover distribution.
        * Generate Random Points: Use a random point generator to create points within each stratum. Ensure that the points are well-distributed and avoid clustering.

        Limitations and Considerations

        * Strata Definition: Defining meaningful strata can be challenging and requires careful consideration of the landscape and available data.
        * Data Availability: The quality and availability of stratification layers can influence the effectiveness of the sampling.
        * Sample Size: Determining the appropriate sample size per stratum is crucial and may require statistical analysis or expert knowledge.
        * Rare Classes: Stratified sampling may not adequately capture rare or sparsely distributed land cover classes. Additional targeted sampling may be needed.
        * Temporal Variability: If significant land cover changes occur throughout the year, consider using imagery from multiple seasons for training data collection.
        * Accessibility: Ensure that the generated sample points are accessible for ground-truthing or high-resolution image interpretation if you plan to validate your training data with field observations.

        Refinement and Validation

        * Visual Inspection: After generating the sample points, visually inspect their distribution on a map to ensure they adequately represent the different strata and land cover types.
        * Ground-Truthing or High-Resolution Imagery: If possible, validate the land cover at the sample points using field visits or high-resolution imagery to confirm the accuracy of your training data.

        By carefully implementing stratified random sampling, you can create a more representative set of training points for your land cover classification model, leading to improved accuracy and generalization across the study area.

        ```python
        import geopandas as gpd
        from shapely.geometry import Point
        from shapely.ops import cascaded_union
        import random

        def stratified_random_sampling(strata_gdf, num_points_per_stratum):
            """
            Generates stratified random sample points.

            Args:
                strata_gdf (GeoDataFrame): GeoDataFrame with strata polygons and a 'stratum' column.
                num_points_per_stratum (dict): Dictionary mapping stratum names to the desired number of points.

            Returns:
                GeoDataFrame: GeoDataFrame with the generated sample points.
            """

            sample_points =
            for stratum in strata_gdf['stratum'].unique():
                stratum_polygon = cascaded_union(strata_gdf[strata_gdf['stratum'] == stratum]['geometry'])
                minx, miny, maxx, maxy = stratum_polygon.bounds

                num_points = num_points_per_stratum.get(stratum, 0)
                for i in range(num_points):
                    while True:
                        x = random.uniform(minx, maxx)
                        y = random.uniform(miny, maxy)
                        point = Point(x, y)
                        if stratum_polygon.contains(point):
                            sample_points.append({'geometry': point, 'stratum': stratum})
                            break

            return gpd.GeoDataFrame(sample_points, crs=strata_gdf.crs)

        # Example usage
        # Assuming you have a GeoDataFrame 'strata_gdf' with your strata
        num_points = {'lowland': 50, 'upland': 30, 'water': 10}  # Example
        sample_points_gdf = stratified_random_sampling(strata_gdf, num_points)

        # Save the sample points
        sample_points_gdf.to_file("sample_points.geojson", driver='GeoJSON')
        ```

There is more detail on how to create strata. Some notes have been generated that raise important points to consider (e.g. what's the required sample size and how are you going to account for temporal variability in the data you are using to generate the strata).

However, the response is still quite vague on the methods used to define the strata. We can go back to AI and prompt it to expand on a method to convert geospatial layers into zones that define the strata.

!!! note "Prompt"
    Can you outline a workflow to convert geospatial layers of vegetation indices, soil type and elevation into groups that represent strata for sampling. Use the k-means algorithm to generate clusters. 

    Generate Python code that fits the k-means algorithm, converts the geospatial layers into a layer of zones defined by the clusters, and vectorise the clusters and store them as a GeoDataFrame.

    ??? note "Response (Gemini 2.0 Flash)"
        Workflow for Stratification using K-means Clustering

        Data Preparation

        * Load your geospatial layers (vegetation indices, soil type, elevation) into appropriate data structures (e.g., NumPy arrays for raster data, GeoDataFrames for vector data).
        * Ensure all layers have the same spatial extent and resolution. If not, resample or clip them to a common grid.
        * Normalize the data for each layer to a common scale (e.g., between 0 and 1) to prevent features with larger values from dominating the clustering process.

        K-means Clustering

        * Choose the number of clusters (k) you want to create. This will determine the number of strata for your sampling. Consider factors like the complexity of the landscape, the diversity of land cover types, and the desired level of detail in your stratification.
        * Apply the k-means algorithm to the combined data from your geospatial layers. The algorithm will group pixels or polygons with similar characteristics into clusters.

        Python code example (using scikit-learn):

        ```python
        import numpy as np
        import pandas as pd
        from sklearn.cluster import KMeans
        from sklearn.preprocessing import MinMaxScaler

        # Assuming you have NumPy arrays: ndvi_array, soil_array, elevation_array
        # Stack the arrays into a single feature matrix
        X = np.dstack([ndvi_array, soil_array, elevation_array]).reshape(-1, 3)

        # Normalize the data
        scaler = MinMaxScaler()
        X_scaled = scaler.fit_transform(X)

        # Apply K-means clustering
        k = 5  # Number of clusters (strata)
        kmeans = KMeans(n_clusters=k, random_state=42)
        clusters = kmeans.fit_predict(X_scaled)

        # Reshape the cluster labels back to the original shape
        cluster_array = clusters.reshape(ndvi_array.shape)
        ```

        Zone Creation

        * Create a new raster layer or vector layer where each pixel or polygon is assigned a zone ID based on its cluster membership.
        * Python code example (using rasterio for raster data):

        ```python
        import rasterio

        # Assuming you have a rasterio dataset 'src' for your original data
        with rasterio.open('strata_raster.tif', 'w', **src.profile) as dst:
            dst.write(cluster_array, 1)
        ```

        Vectorization (Optional)

        * If you prefer working with vector data, convert the raster layer of zones into a polygon shapefile or GeoJSON. This will create distinct polygons representing each stratum.
        * Python code example (using rasterio and geopandas):

        ```python
        import geopandas as gpd
        from rasterio.features import shapes

        # Read the raster data
        with rasterio.open('strata_raster.tif') as src:
            image = src.read(1)
            results = ({'properties': {'zone': v}, 'geometry': s}
                    for i, (s, v) in enumerate(shapes(image, mask=None, transform=src.transform)))

        # Create a GeoDataFrame
        strata_gdf = gpd.GeoDataFrame.from_features(list(results), crs=src.crs)

        # Save the GeoDataFrame
        strata_gdf.to_file("strata_polygons.geojson", driver='GeoJSON')
        ```

        Key Considerations

        * Optimal k Value: Experiment with different values of k to find the optimal number of clusters that best capture the variability in your data. You can use techniques like the elbow method or silhouette analysis to evaluate cluster quality.
        * Cluster Interpretation: Analyze the characteristics of the clusters (e.g., mean values of vegetation indices, soil types, elevation) to understand the environmental conditions they represent. This will help you interpret the results and refine your stratification if needed.
        * Data Normalization: Ensure that you normalize your data appropriately to avoid bias in the clustering process.
        * Spatial Autocorrelation: Consider the potential impact of spatial autocorrelation in your data. If neighboring pixels or polygons are highly correlated, it might influence the clustering results. You can explore spatial clustering techniques if this is a concern.
        * Computational Resources: K-means clustering can be computationally intensive for large datasets. Consider using optimized libraries or cloud computing resources if necessary.

        This workflow provides a structured approach to generate strata for sampling using k-means clustering. Remember to adapt the code and parameters to your specific data and requirements.

With only a handful of prompts we have a workflow in Python code that i) converts several geospatial layers into zones that can be used as strata, and ii) generates a sample of points using these zones as strata. There are also notes explaining each step and points raised for you to consider when implementing this workflow. If you scroll back to the top of this section, you will see through that we have very quickly generated more detailed and useful information to implement the training data generation component of this land cover classification workflow.   



