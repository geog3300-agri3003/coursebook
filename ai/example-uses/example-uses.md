# Example uses of AI tools

These are some examples of prompts and ways to use AI tools. They are designed to give you ideas of how to use AI tools to support and enhance your studies. 

## AI for GIS

**Use the LLM to help you identify GIS operations to solve geospatial problems**

!!! note "Prompt"
    I am a geospatial analyst working for a precision agriculture technology consultancy. I have two drone images of the same field captured on different dates. How can I co-register these images for accurate within-field change analysis? 
    
    Can you consider the strengths and weaknesses of different co-registration methods and outline software tools that support these operations?

    !!! tip "Use thinking models"
        This task requires identifying different co-registration methods and reasoning about their strengths and weakness. "Thinking" models which implement chain-of-thought reasoning and reflect on generated responses are useful for these comparative tasks. The model's "thinking" process will provide a slower and more thoughtful consideration of each co-registration method. 

    ??? note "Response (Gemini 2.0 Flash Thinking)"
        Co-registering drone images captured on different dates is crucial for accurate within-field change analysis in precision agriculture. Misalignment can lead to false change detection, skewing your analysis of vegetation health, growth patterns, or treatment effectiveness. Let's explore the methods, their strengths and weaknesses, and relevant software tools.

        Co-registration Methods for Drone Imagery

        Co-registration aims to spatially align two or more images so that corresponding pixels represent the same ground location. For drone images, especially those of agricultural fields, several methods are applicable, broadly categorized as:

        1. Ground Control Point (GCP) Based Co-registration:

        * Method Description: This is often considered the most accurate and reliable method, especially when high precision is required. It relies on accurately measured ground control points (GCPs) visible in both drone images. GCPs are physical markers placed in the field with known coordinates (typically obtained using a survey-grade GPS or RTK). During image processing, these GCPs are identified in each image and used to geometrically transform one image to match the spatial reference of the other.
        
        Strengths:
        
        * High Accuracy: When GCPs are accurately surveyed and well-distributed, this method provides the highest possible geometric accuracy, crucial for precise change detection.
        * Reliability: GCPs provide absolute spatial reference, making the co-registration robust and less susceptible to image distortions or content changes.
        * Control over Transformation: You can select the type of transformation (e.g., rigid, affine, polynomial) based on the expected distortions and field characteristics.
        
        Weaknesses:

        * Labor-Intensive: Requires physical placement and accurate surveying of GCPs, which can be time-consuming and expensive.
        Field Access Required: Necessitates access to the field on both image acquisition dates to establish or verify GCP locations (if permanent GCPs are not used).
        * GCP Visibility: GCPs must be clearly visible in both drone images, which may require careful marker design and placement, and could be obstructed by vegetation in later image dates.
        * Not always practical for large areas or repeat visits: Placing and surveying GCPs for every image acquisition, especially for large fields or frequent monitoring, can become logistically challenging.
        
        2. Feature-Based (Image-to-Image) Co-registration:

        * Method Description: This method directly aligns images based on automatically detected and matched features within the images themselves. It doesn't rely on external GCPs. Common techniques include:
        * Feature Detection and Matching: Algorithms like SIFT (Scale-Invariant Feature Transform), SURF (Speeded-Up Robust Features), or ORB (Oriented FAST and Rotated BRIEF) are used to identify distinctive features (keypoints) in both images. These features are then matched based on their descriptors.
        * Transformation Estimation: Once enough feature matches are found, a geometric transformation (e.g., affine, projective) is estimated that best aligns the matched features between the images. Robust estimators (like RANSAC) are often used to minimize the influence of outlier matches.
        * Image Resampling: One image is then resampled and warped according to the estimated transformation to align with the reference image.
        
        Strengths:

        * GCP-Free: Eliminates the need for GCPs, significantly reducing fieldwork and time.
        Automated: The process is largely automated, making it more efficient for large datasets.
        * Suitable for areas without GCPs: Applicable even if GCPs were not collected during image acquisition or are impractical.
        * Can adapt to image content changes: Feature-based methods can sometimes be more resilient to slight changes in scene content (e.g., vegetation growth) compared to methods solely relying on initial georeferencing.
        
        Weaknesses:
        
        * Lower Accuracy (generally): Typically less accurate than GCP-based methods, as image features are not as spatially precise as surveyed GCPs. Accuracy depends heavily on image quality, feature distinctiveness, and the quality of feature matching.
        * Potential for Misregistration: In areas with repetitive patterns, textureless surfaces, or significant changes between image dates (e.g., major crop growth, tillage), feature matching can be unreliable, leading to misregistration.
        * Transformation Limitations: Feature-based methods often assume a global transformation model, which might not perfectly correct for localized distortions present in drone imagery (especially from lower-cost drones or in windy conditions).
        * Parameter Tuning: Feature detection and matching algorithms often have parameters that need to be tuned for optimal performance based on image characteristics.
        
        3. Sensor Model/Direct Georeferencing Based Co-registration:

        * Method Description: Modern drones often incorporate high-precision GPS/GNSS and Inertial Measurement Units (IMUs) for direct georeferencing. This means the drone's position and orientation are recorded directly during flight. If both datasets were processed using direct georeferencing (or PPK/RTK post-processing), the images should ideally be already well-aligned based on the sensor model and trajectory data. Co-registration in this case becomes more about fine-tuning.
        
        Strengths:

        * Fast and Efficient: If direct georeferencing is accurate, co-registration can be very fast as the initial alignment is already good.
        * Reduced or Eliminated GCPs (potentially): High-accuracy direct georeferencing (like PPK/RTK) can significantly reduce or even eliminate the need for GCPs in some cases, though GCPs are still recommended for validation and to further improve accuracy, especially for large projects or demanding accuracy requirements.
        Good Starting Point: Provides a good initial geometric correction, simplifying further refinement if needed.
        
        Weaknesses:

        * Accuracy Dependent on Sensor Quality: The accuracy is directly tied to the quality of the drone's GPS/IMU system and the processing method (direct georeferencing vs. PPK/RTK). Lower-cost drones might have less accurate sensors.
        * Atmospheric and Environmental Factors: GPS signals can be affected by atmospheric conditions and obstructions.
        Still may require refinement: Even with direct georeferencing, there can be residual distortions and misalignments. Fine-tuning using feature-based methods or minimal GCPs might still be beneficial for very high-accuracy change analysis.
        * Reliability of Initial Processing: Relies on the accuracy and quality of the initial photogrammetric processing steps that generated the orthomosaics or orthophotos in the first place.
        
        Choosing the Right Method:

        The best co-registration method depends on several factors:

        * Required Accuracy: For very precise within-field change analysis (e.g., centimeter-level accuracy), GCP-based methods are generally preferred. For less stringent accuracy requirements or preliminary assessments, feature-based methods or direct georeferencing refinement might suffice.
        * Availability of GCPs: If GCPs were collected and accurately surveyed, GCP-based methods are a strong choice. If not, you'll need to rely on feature-based or sensor model approaches.
        * Drone and Sensor Quality: Drones with high-precision GPS/IMU systems and processed with PPK/RTK offer better initial georeferencing, potentially reducing the need for extensive co-registration.
        * Image Quality and Content: Image quality (resolution, sharpness, radiometry) and the presence of distinct features significantly impact the success of feature-based methods. Changes in vegetation or scene content between dates can also affect feature matching.
        * Time and Resource Constraints: GCP-based methods are more time and resource-intensive. Feature-based and direct georeferencing refinement are typically faster and require less fieldwork.
        
        Software Tools for Co-registration
        
        Many geospatial software packages support image co-registration. Here are some prominent examples, categorized by type:

        GIS and Remote Sensing Software:

        ArcGIS (Esri):

        * Georeferencing Toolset: Robust tools for GCP-based georeferencing, including various transformation models, residual analysis, and accuracy assessment.
        * Image Analysis Extension: Supports feature-based image registration using techniques like automatic tie point generation and image warping.
        * Workflow: Can integrate GCP-based and feature-based methods.
        * Pros: Widely used in geospatial industry, comprehensive tools, excellent visualization and analysis capabilities.
        * Cons: Commercial software, can be expensive, complex interface for some users.
        
        QGIS (Open Source):

        * Georeferencer GDAL plugin: For GCP-based georeferencing, supports various transformations, allows GCP management, and accuracy assessment.
        * Plugins like "Feature Alignment" or scripting with Python/GDAL/OpenCV: Can be used to implement feature-based co-registration (more advanced, might require programming skills).
        * Pros: Free and open source, versatile, large community support, integrates well with other geospatial libraries.
        * Cons: Feature-based tools might require more manual setup and scripting compared to commercial software, potentially steeper learning curve for complex tasks.
        
        ENVI (Harris Geospatial Solutions):

        * Image Registration Module: Dedicated module for both GCP-based and feature-based image registration. Offers automatic tie point generation, various transformation models, and accuracy metrics.
        * Pros: Specialized for remote sensing image analysis, powerful image processing capabilities, good for handling large datasets.
        * Cons: Commercial software, can be expensive, interface might be less intuitive for general GIS tasks compared to ArcGIS.

        ERDAS IMAGINE (Hexagon Geospatial):

        * Geometric Correction Module: Provides tools for GCP-based georeferencing and image registration, including various transformation options and accuracy reporting.
        * Auto Sync Module: Supports automated feature-based image registration.
        * Pros: Long-standing remote sensing software with strong image processing and photogrammetry capabilities.
        * Cons: Commercial software, can be expensive, interface can be perceived as older and less modern compared to some newer software.
        
        Photogrammetry Software (Often used for drone data processing):

        Agisoft Metashape (formerly PhotoScan):
        
        * Alignment Step: As part of the photogrammetric workflow, Metashape inherently performs image alignment, which can be refined with GCPs or tie points. It's excellent at feature-based alignment for orthomosaic generation and can be used for co-registration of already orthorectified images.
        * Pros: Widely used for drone data processing, strong feature-based alignment capabilities, good for generating orthomosaics and DEMs.
        * Cons: Commercial software, primarily focused on 3D reconstruction workflow, dedicated co-registration tools might be less extensive compared to pure GIS/RS software for specific registration tasks.
        
        Pix4Dmapper/Pix4Dmatic:
        
        * Similar to Metashape: Pix4D products also perform robust image alignment as part of their processing pipeline. Can be used for co-registration and refinement with GCPs or manual tie points.
        * Pros: Popular drone data processing software, good for large datasets, Pix4Dmatic is designed for faster processing of very large datasets.
        * Cons: Commercial software, primarily focused on 3D reconstruction workflow, dedicated co-registration tools might be within the broader processing flow rather than standalone tools.
        
        Programming Libraries (For Custom Solutions and Automation):

        GDAL/OGR (Geospatial Data Abstraction Library):
        
        * Command-line tools and libraries (C++, Python, etc.): Powerful for geospatial data manipulation, including raster warping and transformation. Can be used to implement GCP-based and feature-based co-registration workflows through scripting.
        * Pros: Open source, very versatile, widely used in geospatial development, excellent for batch processing and automation.
        * Cons: Requires programming skills, might have a steeper learning curve for users without programming background, requires assembling workflows from individual functions.
        
        OpenCV (Open Source Computer Vision Library):
        * Python, C++, Java interfaces: Extensive library for computer vision tasks, including feature detection, feature matching, and image transformations. Can be used to build custom feature-based co-registration workflows.
        * Pros: Open source, highly optimized for computer vision tasks, large community, very flexible for custom algorithm development.
        * Cons: Requires programming skills, focused on computer vision primitives, requires more effort to build complete geospatial workflows compared to dedicated GIS/RS software.
        
        Workflow Outline for Co-registration and Change Analysis:

        Pre-processing:

        * Radiometric Calibration: Ensure both images are radiometrically calibrated to minimize the effects of sensor variations and atmospheric conditions on change detection.
        * Orthorectification: If your images are not already orthorectified, process them to generate orthomosaics/orthophotos. This is crucial for accurate geometric alignment and spatial measurements.
        * Resolution Matching (Optional): If the images have significantly different resolutions, you might consider resampling the higher-resolution image to match the lower resolution to avoid artifacts during change analysis.
        * Choose Co-registration Method: Select the most appropriate method based on accuracy requirements, data availability (GCPs), drone/sensor quality, and time constraints (as discussed earlier).

        Perform Co-registration:

        GCP-Based:
        
        * Load GCP coordinates into the software.
        * Identify and mark GCPs in both images.
        * Select a suitable transformation model.
        * Run the georeferencing/co-registration tool.
        
        Feature-Based:
        
        * Select feature detection and matching algorithms in the software.
        * Run the automatic feature matching process.
        * Review and refine matches (remove outliers if necessary).
        * Select a transformation model and run the image registration tool.
        
        Sensor Model Refinement:
        
        If direct georeferencing was used initially, load both orthomosaics into software.
        Consider using feature-based refinement or adding a few well-distributed check points (even without full GCP survey) to fine-tune the alignment.

        Accuracy Assessment:

        * Visual Inspection: Overlay the co-registered images and visually inspect for misalignment, especially in areas with distinct features. "Flicker" or "swipe" tools in GIS software are helpful for this.
        
        Quantitative Accuracy Assessment:
        
        * RMS Error (RMSE): If using GCPs or check points, examine the Root Mean Square Error (RMSE) reported by the software. Lower RMSE values indicate better accuracy.
        * Check Points (Independent): If possible, use independent check points (locations not used in the co-registration process) to assess accuracy.
        * Feature Overlay Analysis: Examine the alignment of features like field boundaries, roads, or drainage ditches after co-registration.
        * Change Analysis: Once you are satisfied with the co-registration accuracy:

        Perform your desired change analysis: This could involve:

        * Band Math/Vegetation Indices Differencing: Calculate differences in NDVI, other vegetation indices, or individual bands to highlight changes in vegetation health, biomass, etc.
        * Image Classification Change Detection: Classify land cover types in both images and analyze changes in classes over time.
        * Object-Based Change Detection: Identify and track changes in individual objects within the field.
        * Interpretation and Validation:  Interpret the change analysis results in the context of agricultural practices, environmental conditions, and your specific research questions. Validate your findings with field observations or other data sources if possible.

        Key Considerations for Precision Agriculture:

        * Within-Field Variability: Agricultural fields often exhibit high within-field variability. Accurate co-registration is essential to ensure that change analysis truly reflects temporal changes and not just spatial misalignment.
        * Sub-Pixel Accuracy: For many precision agriculture applications, achieving sub-pixel co-registration accuracy is desirable to minimize noise and maximize the reliability of change detection.
        * Documentation: Document your co-registration method, parameters, accuracy assessment results, and software tools used for reproducibility and transparency.
        
        By carefully selecting the appropriate co-registration method, utilizing robust software tools, and rigorously assessing accuracy, you can effectively co-register your drone images and perform reliable within-field change analysis for precision agriculture applications. Remember to consider the strengths and weaknesses of each method in the context of your specific project requirements and data characteristics.

!!! note "Prompt"
    I have a series of spatial points with crop yield measurements from a yield monitor on a harvester as a CSV file, a GeoTIFF file storing monthly NDVI values and GeoPackage file storing a field boundary. 
    
    What GIS operations do I need to perform to select all the yield monitor points within the field and to extract the monthly NDVI time-series at each point.

    !!! tip "Help the model generate more accurate and relevant responses"
        The more detail you provide the LLM in your prompt about the task, the more closely the LLM will engage with the specifics of your task and generate a more relevant and useful response. 
        
        The prompt above is a good start describing the formats of the input data. However, it could be improved by specifying what output format you'd like (e.g. structure of a table, visualisations or file format to save the data). You could also ask the model to provide a step by step explantion to ensure it considers each step in the workflow in-turn.

    ??? note "Response (Gemini 2.0 Flash)"
        Load Data

        * Yield Monitor Data: Load the CSV file containing the yield monitor data into a GeoDataFrame. Ensure the data includes latitude and longitude columns or a geometry column representing the point locations.
        * NDVI Data: Load the GeoTIFF file containing monthly NDVI values as a raster dataset.
        * Field Boundary: Load the GeoPackage file and extract the field boundary polygon.
        
        Spatial Selection

        * Reproject (if necessary): Ensure that the yield monitor data and the field boundary are in the same coordinate reference system (CRS). If not, reproject one of the datasets to match the other.
        * Spatial Join: Perform a spatial join between the yield monitor points and the field boundary polygon. Use the 'intersects' or 'within' spatial predicate to select only the points that fall within the field boundary. This will create a new GeoDataFrame containing only the yield points within the field.
        
        Time-Series Extraction

        * Reproject (if necessary): Ensure that the yield monitor points (now filtered to be within the field) and the NDVI raster are in the same CRS. If not, reproject one of the datasets to match the other.
        * Extract NDVI Values: For each point in the filtered yield monitor data, extract the corresponding NDVI value from each monthly layer in the NDVI raster. This will create a new DataFrame or extend the existing GeoDataFrame with columns representing the NDVI values for each month.

        Python Code Example (using geopandas and rasterio)

        ```python
        import geopandas as gpd
        import rasterio

        # Load data
        yield_data = gpd.read_file("yield_monitor.csv")  # Replace with your file name
        ndvi_raster = rasterio.open("monthly_ndvi.tif")  # Replace with your file name
        field_boundary = gpd.read_file("field_boundary.gpkg", layer="field_layer")  # Replace with your file name and layer name

        # Reproject if necessary (ensure yield_data and field_boundary have the same CRS)
        yield_data = yield_data.to_crs(field_boundary.crs)

        # Spatial join
        yield_within_field = gpd.sjoin(yield_data, field_boundary, how="inner", predicate="intersects")

        # Reproject if necessary (ensure yield_within_field and ndvi_raster have the same CRS)
        yield_within_field = yield_within_field.to_crs(ndvi_raster.crs)

        # Extract NDVI time-series
        for i in range(1, ndvi_raster.count + 1):
            ndvi_values = [ndvi_raster.read(i).item(int(x), int(y)) for x, y in zip(yield_within_field.geometry.x, yield_within_field.geometry.y)]
            yield_within_field[f"NDVI_month_{i}"] = ndvi_values

        # Save or use the resulting GeoDataFrame
        print(yield_within_field.head())
        ```
        
        Explanation

        * The code first loads the yield data, NDVI raster, and field boundary into appropriate data structures.
        * It then reprojects the yield data to match the CRS of the field boundary (if necessary) and performs a spatial join to select points within the field.
        * Next, it reprojects the filtered yield data to match the CRS of the NDVI raster (if necessary).
        * It iterates through each band (month) in the NDVI raster, extracts the NDVI value at each point location, and adds it as a new column in the GeoDataFrame.
        * The resulting GeoDataFrame contains the yield data for points within the field, along with their corresponding monthly NDVI values.
        
        Additional Considerations

        * Point in Polygon: The spatial join operation effectively performs a "point-in-polygon" test to determine which points are inside the field boundary.
        * Raster Value Extraction: The code extracts the NDVI value at the exact pixel location of each point. You can explore other extraction methods (e.g., bilinear interpolation) if needed.
        * Data Cleaning: Check for missing or invalid values in both the yield data and the NDVI time-series.
        * Analysis: Once you have the combined data, you can perform further analysis, such as correlating yield with NDVI trends or building predictive models.

!!! note "Prompt"
    I have a dataset of tree locations represented as spatial points and field boundaries in a separate polygon layer. How can I count the number of trees within each field? 
    
    Please provide a step by step explanation. 

    ??? note "Response (Gemini 2.0 Flash)"
        You can count the number of trees within each field using a spatial join or intersection operation.  Here's a step-by-step explanation, applicable to most GIS software (like QGIS, ArcGIS, or PostGIS):

        Prepare your data:

        * Tree Points: Ensure your tree locations are in a point layer (e.g., shapefile, GeoJSON, or a spatial database table). Each point should represent a single tree. The crucial part is that these points have a defined coordinate system (CRS).
        * Field Polygons: Make sure your field boundaries are in a polygon layer (e.g., shapefile, GeoJSON, or a spatial database table). Each polygon represents a single field. This layer must be in the same coordinate system as your tree point layer. If they aren't, you'll need to reproject one of the layers to match the other before proceeding. Consistent CRS is absolutely essential for accurate spatial analysis.
        
        Perform the Spatial Join (or Intersection):

        This is the core step. The specific tool name might vary slightly depending on your GIS software, but the concept is the same.  You're essentially asking the GIS to determine which trees fall within which fields.

        QGIS: Use the "Join attributes by location" tool (found in the Vector menu under "Spatial join").
        
        * Target layer: Your field polygons.
        * Join layer: Your tree points.
        * Join type: "One to many" (because one field can contain multiple trees).
        * Output layer: A new polygon layer. This new layer will have all the attributes of your original field polygons, plus information about the trees within each field (specifically, the count).

        ArcGIS: Use the "Spatial Join" tool.
        
        * Target Features: Your field polygons.
        * Join Features: Your tree points.
        * Join Type: "One to Many".
        * Match Option: "Within" (or "Contains," depending on how you define "within").
        * Output Feature Class: A new feature class (polygon layer).
        
        PostGIS (SQL): Use a spatial query with the ST_Contains or ST_Intersects function. Here's a basic example:

        ```
        CREATE TABLE field_tree_counts AS
        SELECT
            fields.field_id,  -- Replace with your field's unique ID column
            fields.field_name, -- Replace with your field's name column (optional)
            COUNT(trees.tree_id) AS tree_count -- Replace with your tree's unique ID column
        FROM
            fields_table AS fields, -- Replace with your fields table name
            trees_table AS trees  -- Replace with your trees table name
        WHERE
            ST_Contains(fields.geom, trees.geom) -- Or ST_Intersects(fields.geom, trees.geom)
        GROUP BY
            fields.field_id, fields.field_name; -- Group by field ID and name
        ```
        
        View the Results:

        The output from the spatial join will be a new polygon layer. This layer will have the attributes of your original field polygons. Crucially, it will also have a new attribute (usually named something like tree_count or COUNT) that represents the number of trees within each field.
        
        Open the attribute table of this new layer to see the counts.
        
        (Optional) Further Analysis:

        You can now use this new layer for further analysis, like calculating statistics (average tree density per field), visualizing the results (e.g., choropleth maps showing tree density), or exporting the data to other formats.
        
        Key Considerations:

        * Coordinate Systems (CRS): Double-check that your tree and field data are in the same CRS. This is the most common source of errors in spatial analysis.
        * "Within" vs. "Contains": In some GIS software, you might have the option of "Within" or "Contains." "Within" means the tree's entire geometry must be within the field. "Contains" means the field contains the tree. Usually, "Within" is the more appropriate choice for this type of analysis.
        * Data Cleaning: Before performing the spatial join, it's a good idea to check your data for any errors, like overlapping polygons or invalid geometries. Cleaning your data can prevent unexpected results.
        * Performance: If you have a very large dataset, the spatial join can be computationally intensive. Consider using spatial indexing or optimizing your query (if using a spatial database) to improve performance.
        
        By following these steps, you should be able to accurately count the number of trees within each field in your dataset.  Remember to adapt the instructions slightly depending on the specific GIS software you are using.

**Describe a geospatial problem and your datasets and instruct the LLM to give you detailed instructions on how to solve the problem using desktop GIS (e.g. QGIS / ArcGIS)**

!!! note "Prompt"
    I have a series of spatial points with crop yield measurements from a yield monitor on a harvester as a CSV file, a GeoTIFF file storing monthly NDVI values and GeoPackage file storing a field boundary. I need to select all the yield monitor points within the field and to extract the monthly NDVI time-series at each point. 
    
    Can you provide detailed step-by-step instructions to do this using QGIS?

    ??? "Response (Gemini 2.0 Flash)"
        Here's a detailed step-by-step guide on how to select yield points within a field boundary and extract monthly NDVI time series at each point using QGIS:

        Load Your Data:

        Yield Points (CSV):
        
        * Open QGIS.
        * Go to Layer -> Add Layer -> Add Delimited Text Layer.
        * Browse to your CSV file.
        * Ensure the correct delimiter (comma in your case) is selected.
        * Specify the X and Y fields (longitude and latitude).
        * Set the CRS (Coordinate Reference System) to match your other data. This is crucial. If you're unsure, check the properties of your GeoTIFF or field boundary. Common CRS for agricultural data include UTM zones or geographic coordinates (WGS 84).
        * Click Add. The points should now appear on the map.
        
        NDVI GeoTIFF:

        * Go to Layer -> Add Layer -> Add Raster Layer.
        * Browse to your GeoTIFF file.
        * Click Add. The NDVI raster should now be displayed. It's likely that you will need to repeat this step for each monthly NDVI GeoTIFF you want to extract data from.
        
        Field Boundary (GeoPackage):
        
        * Go to Layer -> Add Layer -> Add Vector Layer.
        * Browse to your GeoPackage file.
        * Select the field boundary layer.
        * Click Add. The field boundary should now be visible.
        
        
        Select Yield Points within the Field:

        * Go to Vector -> Geoprocessing Tools -> Clip.
        * Input layer: Your yield points layer.
        * Overlay layer: Your field boundary layer.
        * Output layer: Choose a location and name for the new clipped points layer (e.g., yield_points_in_field.shp).
        * Click Run. This will create a new point layer containing only the yield points that fall within the field boundary.
        
        Extract NDVI Values at Each Point (for one month):

        * Go to Plugins -> Manage and Install Plugins.
        * Search for and install the "Point sampling tool" plugin if you don't have it already.
        * Go to Plugins -> Point sampling tool -> Point sampling tool.
        * Input point layer: Your clipped yield points layer (yield_points_in_field).
        * Input raster layer: Your NDVI GeoTIFF (for the specific month).
        * Output layer: Choose a location and name for the output (e.g., yield_points_ndvi_month1.shp).
        * Click Run. This will create a new point layer. The attribute table of this new layer will contain the NDVI value (from the raster) at each point. The name of the new field may be the same as the raster file name.
        
        Repeat for all months:

        Repeat step 3 for each of your monthly NDVI GeoTIFF files. Make sure to create a new output file for each month (e.g., yield_points_ndvi_month2.shp, yield_points_ndvi_month3.shp, etc.).
        
        Combine Monthly Data (Optional but usually needed):

        You now have multiple point layers, each containing the NDVI values for a different month.  To create a single table with the time series data, you can use the "Join attributes by location" tool (Vector -> Data Management Tools -> Join attributes by location) or the "Merge vector layers" tool (Vector -> Data Management Tools -> Merge vector layers).  The latter is easier to use, but requires some data manipulation to get it into a time series format.

        Using "Merge vector layers" (Recommended):

        * Merge all the monthly point layers into a single layer.
        * Open the attribute table of the merged layer. You'll now have all the monthly NDVI values as different columns.
        * Export the attribute table to a CSV file. This will be easier to work with for time series analysis.
        * Using "Join attributes by location" (More complex):
        * You can join each month's NDVI data to the original yield points layer. This will create separate layers for each month.
        * You can then merge these layers or export the attribute tables to create your time series dataset.
        
        Time Series Analysis (Outside QGIS):

        Once you have your data in a CSV file, you'll likely want to perform time series analysis or visualization. You can use tools like R, Python (with libraries like Pandas and Matplotlib), or even Excel for this.
        
        Important Considerations:

        * Coordinate Systems (CRS): Absolutely critical. Make sure all your layers (yield points, NDVI rasters, and field boundary) are in the same CRS. If they're not, you must reproject them before doing any spatial analysis.
        * NDVI Raster Alignment: Ensure your monthly NDVI rasters align spatially. If they are slightly misaligned, you might get incorrect NDVI values at your points.
        * Data Volume: If you have a very large dataset, these operations might take some time. Consider using a spatial database (like PostGIS) for better performance.
        * QGIS Versions: The menu options might be slightly different depending on your QGIS version, but the general workflow should be the same.
        
        This comprehensive guide should help you extract the NDVI time series for your yield points. Remember to double-check your CRS at every step!  If you encounter specific errors, please provide the error message and details about your data, and I'll do my best to assist you further.

**Screen share with Google AI Studio and ask it questions on where to find tools and features in the QGIS user interface as you work in a desktop GIS** 

Using <a href="https://aistudio.google.com" target="_blank">Google AI Studio</a> you can screen share QGIS with Gemini and verbally ask questions for help with completing tasks and navigating the user interface; for example, where to find processing tools and how to configure them for GIS operations.

<iframe src="https://drive.google.com/file/d/1o8mdSEH_OFbLIU8hAaltmd1Ui7qm-2Zl/preview" width="640" height="480" allow="autoplay"></iframe>

*Screen share of QGIS with Google AI Studio to get instructions on a workflow to turn raster pixels into a polygon layer.*

## AI for programming and data analysis

**Use LLMs for programming from writing boilerplate code, generating examples and helping solve coding problems**

!!! note "Prompt"
    Can you write a Python function to merge three tables using a common ID column?

    ??? note "Response (Gemini 2.0 Flash)"

        ```python
        import pandas as pd

        def merge_tables(table1, table2, table3, common_id):
            """
            Merges three pandas DataFrames using a common ID column.

            Args:
                table1: The first pandas DataFrame.
                table2: The second pandas DataFrame.
                table3: The third pandas DataFrame.
                common_id: The name of the common ID column (string).

            Returns:
                A new pandas DataFrame containing the merged data, or None if an error occurs.
                Prints informative messages if an issue is encountered.
            """

            try:
                # Check if the common ID column exists in all tables
                if common_id not in table1.columns or common_id not in table2.columns or common_id not in table3.columns:
                    print(f"Error: Common ID column '{common_id}' not found in all tables.")
                    return None

                # Merge the tables
                merged_table = pd.merge(table1, table2, on=common_id, how='outer')  # Outer join to keep all rows
                merged_table = pd.merge(merged_table, table3, on=common_id, how='outer')

                return merged_table

            except Exception as e:
                print(f"An error occurred during merging: {e}")
                return None


        # Example Usage:
        # Create sample DataFrames (replace with your actual data)
        data1 = {'ID': [1, 2, 3, 4], 'Value1': ['A', 'B', 'C', 'D']}
        table1 = pd.DataFrame(data1)

        data2 = {'ID': [2, 3, 4, 5], 'Value2': ['E', 'F', 'G', 'H']}
        table2 = pd.DataFrame(data2)

        data3 = {'ID': [1, 3, 5, 6], 'Value3': ['I', 'J', 'K', 'L']}
        table3 = pd.DataFrame(data3)

        common_id_column = 'ID'

        merged_data = merge_tables(table1, table2, table3, common_id_column)

        if merged_data is not None:
            print(merged_data)  # Print the merged DataFrame
            # You can now save it to a new file, perform further operations, etc.
            # For example, to save to a CSV:
            # merged_data.to_csv("merged_data.csv", index=False)
        ```

        Explanation and Improvements:

        * Error Handling: The code includes a try...except block to catch potential errors during the merge process. This makes the function more robust. It also checks if the common_id column actually exists in all tables before attempting the merge.
        * Clearer Error Messages: The print() statements provide more informative messages if there's an issue, making debugging easier.
        * Outer Join: The how='outer' parameter in pd.merge() performs an outer join. This is crucial. An outer join ensures that all rows from all tables are included in the merged table. If a particular ID is missing in one of the tables, the corresponding columns for that table will have NaN (Not a Number) values. This prevents data loss.
        * Example Usage: The example code demonstrates how to use the function with sample DataFrames. Replace these sample DataFrames with your actual data. It also shows how to save the result to a CSV file.
        * Conciseness: The code is more concise and readable.
        * Flexibility: The function is more flexible, as it can handle cases where one or more of the DataFrames might not contain all the IDs present in the others.

        How to Use with Your Data:

        
        Import pandas: 
        
        ```python
        import pandas as pd
        ```
        
        Read your tables: Use pd.read_csv(), pd.read_excel(), or other appropriate pandas functions to read your three tables into pandas DataFrames. For example:

        ```python
        table1 = pd.read_csv("table1.csv")
        table2 = pd.read_csv("table2.csv")
        table3 = pd.read_csv("table3.csv")
        ```

        Call the function:

        ```python
        merged_data = merge_tables(table1, table2, table3, "ID")  # Replace "ID" with your actual common ID column name
        ```

        Handle the result:

        ```python
        if merged_data is not None:
            print(merged_data)
            merged_data.to_csv("merged_result.csv", index=False) # Save to a CSV
        ```

        Remember to replace "table1.csv", "table2.csv", "table3.csv", and "ID" with the actual paths to your files and the name of your common ID column.`

!!! note "Prompt"
    Can you write Python code snippet to open a NetCDF file and print information about the file’s contents?

    !!! tip "Use LLMs to help you work with different file formats"
        A wide range of file formats are used to store and transfer geospatial and agricultural data (e.g. see the list of raster file formats supported by <a href="https://gdal.org/en/stable/drivers/raster/index.html" target="_blank">GDAL</a>). It's not always obvious how data is stored in different file formats or how to read data from them. 
        
        LLMs can be a useful tool to give you quick examples of how to read data from a new format and provide an introduction to working with file formats that might be more accessible than their technical documentation. 

        Often, there is more than one way to read data from a file format. You could improve this prompt by asking the model to provide examples of different ways to open a NetCFD file and comment on their strengths and weaknesses. 

    ??? note "Response (Gemini 2.0 Flash)"

        ```python
        import netCDF4  # Import the netCDF4 library

        def print_netcdf_info(filepath):
            """Opens a NetCDF file and prints information about its contents.

            Args:
                filepath: The path to the NetCDF file (string).
            """

            try:
                # Open the NetCDF file in read mode ('r')
                with netCDF4.Dataset(filepath, 'r') as nc_file:  # Use 'with' for automatic closing

                    print(f"NetCDF file: {filepath}")
                    print("=" * 40)

                    # Print global attributes (metadata)
                    print("\nGlobal Attributes:")
                    for attr_name in nc_file.ncattrs():
                        print(f"  {attr_name}: {nc_file.getncattr(attr_name)}")

                    # Print dimensions
                    print("\nDimensions:")
                    for dim_name, dim in nc_file.dimensions.items():
                        print(f"  {dim_name}: {len(dim)}")

                    # Print variables
                    print("\nVariables:")
                    for var_name, var in nc_file.variables.items():
                        print(f"  {var_name}:")
                        print(f"    Dimensions: {var.dimensions}")
                        print(f"    Shape: {var.shape}")
                        print(f"    Data type: {var.dtype}")
                        print(f"    Attributes:")
                        for attr_name in var.ncattrs():
                            print(f"      {attr_name}: {var.getncattr(attr_name)}")
                        # Print a few values (optional - for small variables only!)
                        if var.size < 100:  # Avoid printing huge arrays
                            print(f"    Values: {var[:]}")  # [:] gets all data

            except FileNotFoundError:
                print(f"Error: NetCDF file not found at {filepath}")
            except Exception as e:
                print(f"An error occurred: {e}")

        # Example usage:
        filepath = "your_netcdf_file.nc"  # Replace with the actual path to your NetCDF file
        print_netcdf_info(filepath)

        # Example of reading data from a specific variable:
        def read_netcdf_variable(filepath, variable_name):
            try:
                with netCDF4.Dataset(filepath, 'r') as nc_file:
                    if variable_name in nc_file.variables:
                        variable_data = nc_file.variables[variable_name][:]  # Read all data
                        return variable_data
                    else:
                        print(f"Variable '{variable_name}' not found in the NetCDF file.")
                        return None
            except Exception as e:
                print(f"An error occurred: {e}")
                return None

        # Example usage of reading a variable
        temperature_data = read_netcdf_variable(filepath, "temperature") # Replace "temperature" with your variable name
        if temperature_data is not None:
            print(temperature_data.shape) # Print the shape of the data
            print(temperature_data) # Print the data (optional)
        ```

        Key Improvements and Explanations:

        * netCDF4 Library: The code uses the netCDF4 library, which is the recommended way to work with NetCDF files in Python. You'll likely need to install it: pip install netCDF4
        * Error Handling: The `try...except` block handles potential errors, such as the file not being found or other issues during file processing. This makes the code more robust.
        * `with` Statement: The `with netCDF4.Dataset(...) as nc_file:` construct ensures that the NetCDF file is automatically closed, even if errors occur. This is best practice.
        * Clear Output: The code prints information about global attributes, dimensions, and variables in a well-formatted way.
        Variable Information: For each variable, it prints the dimensions, shape, data type, and attributes.
        * Optional Data Printing: It includes an option to print a few values from the variables. However, this is only done if the variable is relatively small (less than 100 elements) to avoid printing huge arrays to the console.
        * Example of reading a variable: The `read_netcdf_variable` function demonstrates how to read the data from a specific variable in a NetCDF file. This is how you would actually access the data for analysis.
        * Comments: The code is well-commented to explain what each part does.
        
        How to Use:

        * Install netCDF4: `pip install netCDF4`
        * Replace `your_netcdf_file.nc`: Change `"your_netcdf_file.nc"` to the actual path of your NetCDF file.
        * Run the script: Execute the Python script. It will print information about the NetCDF file's contents.
        * Access Variable Data: Use the read_netcdf_variable function to get the data for the variables you need. Remember to replace `"temperature"` with the actual name of the variable you want. You can then work with the data (e.g., plot it, perform calculations, etc.).

**Use LLMs to improve or optimise code**

!!! note "Prompt"
    From the perspective of a senior developer reviewing code, can you suggest how to improve the following Python function?

    The two variables `precip_jan_2024` and `precip_jan_2025` are 3D arrays of shape (time, X, Y) storing precipitation daily precipitation values. The code is trying to create monthly sums of precipitation for each pixel and then compute the change in precipitation between 2024 and 2025.

    ```python
    precip_jan_2024_sum = np.zeros((precip_jan_2024.shape[1], precip_jan_2024.shape[2]))
    
    for r in range(0, precip_jan_2024.shape[1]):
        for c in range(0, precip_jan_2024.shape[2]):
            precip_jan_2024_sum[r, c] = np.sum(precip_jan_2024[:, r, c])

    precip_jan_2025_sum = np.zeros((precip_jan_2025.shape[1], precip_jan_2025.shape[2]))

    for r in range(0, precip_jan_2025.shape[1]):
        for c in range(0, precip_jan_2025.shape[2]):
            precip_jan_2025_sum[r, c] = np.sum(precip_jan_2025[:, r, c])

    diff = np.zeros((precip_jan_2025_sum.shape[1], precip_jan_2025_sum.shape[2]))

    for r in range(0, precip_jan_2024_sum.shape[1]):
        for c in range(0, precip_jan_2024_sum.shape[2]):
            diff[r, c] = precip_jan_2025_sum[r, c] - precip_jan_2024_sum[r, c]
    ```

    ??? note "Response (Gemini 2.0 Flash)"

        ```python
        import numpy as np

        def calculate_precipitation_change(precip_jan_2024, precip_jan_2025):
            """Calculates the change in monthly precipitation between two years.

            Args:
                precip_jan_2024: 3D numpy array (time, X, Y) of daily precipitation 
                                values for January 2024.
                precip_jan_2025: 3D numpy array (time, X, Y) of daily precipitation 
                                values for January 2025.  Must have the same X and Y
                                dimensions as precip_jan_2024.

            Returns:
                A 2D numpy array representing the difference in total monthly 
                precipitation between January 2025 and January 2024 for each 
                grid point (X, Y).  Returns None if the input arrays have incompatible
                shapes.
            """

            if precip_jan_2024.shape[1:] != precip_jan_2025.shape[1:]:  # Check X, Y dimensions
                print("Error: Precipitation arrays have incompatible X and Y dimensions.")
                return None


            precip_jan_2024_sum = np.sum(precip_jan_2024, axis=0)  # Sum along the time axis
            precip_jan_2025_sum = np.sum(precip_jan_2025, axis=0)

            diff = precip_jan_2025_sum - precip_jan_2024_sum

            return diff


        # Example (replace with your actual data):
        precip_2024 = np.random.rand(31, 10, 10)  # 31 days, 10x10 grid
        precip_2025 = np.random.rand(31, 10, 10)

        precipitation_diff = calculate_precipitation_change(precip_2024, precip_2025)

        if precipitation_diff is not None:
            print(precipitation_diff)
        ```

        Key Improvements and Explanations:

        * Vectorization: The original code used nested loops to iterate over the X and Y dimensions. This is very inefficient in Python, especially with NumPy.  The improved code uses NumPy's vectorized operations (np.sum with the axis parameter) to perform the summations and difference calculation.  Vectorization is orders of magnitude faster.
        * Function: Encapsulating the code within a function (calculate_precipitation_change) makes it reusable, more organized, and easier to test.
        * Input Validation: The function now checks if the X and Y dimensions of the input arrays are compatible. This is a crucial step for robust code.  It returns None and prints an informative message if the shapes don't match.
        * Clearer Variable Names:  Slightly more descriptive variable names (e.g., precip_jan_2024_sum instead of just sum2024) improve readability.
        * Docstring: The function includes a docstring explaining its purpose, arguments, and return value. This is essential for maintainability and collaboration.
        * Removed Redundant Initialization: The original code initialized precip_jan_2024_sum, precip_jan_2025_sum and diff with zeros. This is not needed when using np.sum with the axis argument.
        * Direct Calculation: The difference is now calculated directly using NumPy's broadcasting.  This is another example of vectorization and is much more efficient than looping.

        Why Vectorization is Crucial:

        NumPy's vectorized operations are implemented in C and are highly optimized.  When you use loops in Python to perform numerical calculations on arrays, you're essentially iterating through the array elements in Python, which is slow.  Vectorization allows NumPy to perform the operations on the entire array (or a specified axis) at once, using optimized C code under the hood. The performance difference can be enormous, especially for large datasets.  Always try to vectorize your NumPy code whenever possible.

!!! note "Prompt"
    From the perspective of a senior developer reviewing code, can you suggest how to optimise the following Python script? 
    
    Can you explain how these optimisations work and are suitable for this program?

    ```python
    import pystac
    import planetary_computer as pc
    import rioxarray  # For easier raster handling
    import xarray as xr

    # get Sentinel-2 images from Planetary Computer
    def get_sentinel2_images_unoptimized(bbox, start_date, end_date, cloud_cover_max=30):
        """Retrieves Sentinel-2 images from Planetary Computer."""
        catalog = pystac.Catalog.from_file_stac_catalog(pc.CATALOG_URL)

        items = catalog.search(
            bbox=bbox,
            datetime=[start_date, end_date],
            collections=["sentinel-2-l2a"],  # Or appropriate collection
        ).items()

        image_data = []  # List to hold image data (inefficient)

        for item in items:
            if item.properties["eo:cloud_cover"] <= cloud_cover_max:
                asset = item.assets["image"] # Get the image asset
                href = asset.href
                try:
                    # Open the raster data
                    raster = rioxarray.open_rasterio(href, masked=True).squeeze() # Squeeze to remove extra dimensions
                    image_data.append(raster) # Append to a list - inefficient!
                except Exception as e:
                    print(f"Error processing item {item.id}: {e}")

        if not image_data:
            return None

        combined_data = xr.concat(image_data, dim="time") # Concatenate the list of xarray DataArrays

        return combined_data
    ```
    ??? note "Response (Gemini 2.0 Flash)"
        This code snippet retrieves Sentinel-2 images from the Planetary Computer using the pystac library for searching the STAC catalog and rioxarray for loading the images as xarray DataArrays. Here's an optimized version with explanations:

        ```python
        import pystac
        import planetary_computer as pc
        import rioxarray
        import xarray as xr

        def get_sentinel2_images_optimized(bbox, start_date, end_date, cloud_cover_max=30):
            """Retrieves Sentinel-2 images from Planetary Computer (optimized)."""

            catalog = pystac.Catalog.from_file_stac_catalog(pc.CATALOG_URL)

            search = catalog.search(
                bbox=bbox,
                datetime=[start_date, end_date],
                collections=["sentinel-2-l2a"],
                query={"eo:cloud_cover": {"lt": cloud_cover_max}}  # Filter by cloud cover
            )

            items = search.get_items()  # Retrieve the items

            # Use a generator expression for efficient filtering and data loading
            image_data_generator = (
                rioxarray.open_rasterio(item.assets["image"].href, masked=True).squeeze()
                for item in items
            )

            try:
                # Concatenate directly from the generator (memory-efficient)
                combined_data = xr.concat(image_data_generator, dim="time")
                return combined_data
            except ValueError:  # Handle the case where no images are found
                print("No Sentinel-2 images found matching the criteria.")
                return None
            except Exception as e:  # Catch other potential errors during processing
                print(f"An error occurred: {e}")
                return None
        ```

        Key Improvements and Explanations:

        * Cloud Cover Filtering in STAC Query: The optimized version filters the images by cloud cover directly in the STAC query using the query parameter. This reduces the number of items retrieved and processed, improving efficiency.
        * Generator Expression: Instead of appending each image to a list and then concatenating, the optimized version uses a generator expression within the xr.concat function. This avoids loading all the images into memory at once, which is crucial when dealing with large datasets.
        * Direct Concatenation: Concatenating directly from the generator is more memory-efficient than creating an intermediate list of xarray DataArrays.
        * Error Handling: The optimized version includes a try...except block to handle potential errors during file opening or concatenation. It also specifically handles the ValueError that can occur if xr.concat is called with an empty iterable (i.e., no images are found).

        Why these changes are important:

        * Memory Efficiency: The generator expression and direct concatenation significantly reduce memory usage, especially when dealing with many or large images.
        * Performance: The optimized version is likely to be faster due to reduced data retrieval and more efficient concatenation.
        * Robustness: Error handling makes the code more reliable and prevents crashes due to unexpected issues.
        * Best Practices: Using generator expressions, direct concatenation, and proper error handling are considered best practices in Python, making your code more efficient, reliable, and maintainable.
        
        This optimized version demonstrates how to efficiently retrieve and process Sentinel-2 images from the Planetary Computer while adhering to best practices for memory management and error handling.


**Use LLMs to tidy up, style and document code**

!!! note "Prompt"
    Can you add documentation to the following Python code? It is a function that computes the maximum NDVI value in a time-series with the data stored in a 3D array. 

    ```python
    import numpy as np

    def estimate_max_ndvi_3d(ndvi_array):
        max_ndvi_values = np.nanmax(ndvi_array, axis=0)
        return max_ndvi_values
    ```

    ??? note "Response (ChatGPT)"

        ```python
        import numpy as np

        def estimate_max_ndvi_3d(ndvi_array):
            """
            Computes the maximum NDVI (Normalized Difference Vegetation Index) value over time.

            This function takes a 3D NumPy array representing NDVI values recorded over time 
            and computes the maximum NDVI value at each spatial location while ignoring NaN values.

            Parameters
            ----------
            ndvi_array : numpy.ndarray
                A 3D array of NDVI values with shape (time, height, width), where:
                - `time` represents different time points in the series.
                - `height` and `width` correspond to the spatial dimensions.

            Returns
            -------
            numpy.ndarray
                A 2D array of maximum NDVI values with shape (height, width), containing 
                the highest NDVI value observed at each spatial location over time.

            Notes
            -----
            - The function uses `numpy.nanmax` to ignore NaN values when computing the maximum.
            - If all values for a given spatial location are NaN, the result will be NaN at that location.

            Example
            -------
            >>> import numpy as np
            >>> ndvi_data = np.array([
            ...     [[0.2, 0.4], [0.1, np.nan]],
            ...     [[0.5, 0.3], [0.6, 0.7]],
            ...     [[0.3, 0.6], [np.nan, 0.8]]
            ... ])
            >>> estimate_max_ndvi_3d(ndvi_data)
            array([[0.5, 0.6],
                [0.6, 0.8]])
            """
            max_ndvi_values = np.nanmax(ndvi_array, axis=0)  # Compute max along time axis, ignoring NaNs
            return max_ndvi_values
        ```

        Improvements
        
        * Added a detailed docstring that follows the NumPy documentation style.
        * Explained input and output formats for better usability.
        * Included an example to demonstrate how the function works.
        
        This makes the function easier to understand, maintain, and use in larger projects. 🚀


!!! note "Prompt"
    From the perspective of a senior developer reviewing code, can you suggest how to improve the clarity of the following Python code? It is a Python function that generates management zones from a multispectral UAV image and a soil map.

    ```python 
    import numpy as np
    from sklearn.cluster import KMeans

    def generate_management_zones(uav_image, soil_map, num_zones):
        uav_reshaped = uav_image.reshape(-1, uav_image.shape[2])
        soil_reshaped = soil_map.flatten().reshape(-1, 1)
        combined_data = np.hstack((uav_reshaped, soil_reshaped))
        kmeans = KMeans(n_clusters=num_zones, random_state=42)
        kmeans.fit(combined_data)
        zones = kmeans.labels_.reshape(uav_image.shape[:2])
        return zones
    ```

    ??? note "Response (ChatGPT)"

        ```python
        import numpy as np
        from sklearn.cluster import KMeans

        def generate_management_zones(multispectral_image, soil_data, num_clusters):
            """
            Generate management zones for a field using k-means clustering on multispectral UAV imagery and soil data.

            Parameters:
            - multispectral_image: numpy.ndarray
                A 3D array representing the multispectral UAV image with dimensions (height, width, num_bands).
            - soil_data: numpy.ndarray
                A 2D array representing the soil map with dimensions (height, width).
            - num_clusters: int
                The desired number of management zones (clusters).

            Returns:
            - management_zones: numpy.ndarray
                A 2D array with the same spatial dimensions as the input data, where each element indicates the assigned management zone.
            """
            # Reshape the multispectral image to a 2D array where each row is a pixel and each column is a spectral band
            pixels = multispectral_image.reshape(-1, multispectral_image.shape[2])

            # Flatten the soil data to create a 1D array of soil values
            soil_values = soil_data.flatten().reshape(-1, 1)

            # Combine the spectral and soil data into a single feature array
            features = np.hstack((pixels, soil_values))

            # Initialize the k-means clustering algorithm with the specified number of clusters
            kmeans = KMeans(n_clusters=num_clusters, random_state=42)

            # Fit the k-means algorithm to the combined feature data
            kmeans.fit(features)

            # Retrieve the cluster labels for each pixel and reshape them to match the original image dimensions
            management_zones = kmeans.labels_.reshape(multispectral_image.shape[:2])

            return management_zones
        ```

        Explanation:

        Reshaping the Multispectral Image:

        * The multispectral UAV image is a 3D array with dimensions corresponding to height, width, and the number of spectral bands.
        * We reshape this array into a 2D array (`pixels`) where each row represents a pixel, and each column corresponds to a spectral band. This transformation is necessary for clustering, as k-means operates on 2D data matrices.
        
        Flattening the Soil Data:
        
        * The soil map is a 2D array with spatial dimensions matching the multispectral image.
        * We flatten this array into a 1D array (`soil_values`) to align each soil measurement with the corresponding pixel in the multispectral image.
        
        Combining Features:
        
        * We horizontally stack (`np.hstack`) the spectral data (`pixels`) and the soil data (`soil_values`) to form a single feature matrix (`features`). Each row in this matrix represents a pixel with its associated spectral and soil attributes.
        
        Applying K-Means Clustering:
        
        * We initialize the k-means algorithm with the desired number of clusters (`num_clusters`) and a fixed random state for reproducibility.
        * The `fit` method is used to perform clustering on the combined feature matrix.
        
        Reshaping Cluster Labels:
        
        * The `labels_` attribute of the fitted k-means model contains the cluster assignments for each pixel.
        * We reshape these labels back into a 2D array (`management_zones`) that matches the spatial dimensions of the original multispectral image, where each element indicates the assigned management zone.
        
        This function effectively integrates multispectral imagery and soil data to delineate distinct management zones within a field, facilitating targeted agricultural practices.



**Use LLMs to help you retrieve data from, or upload data to, web APIs and online data providers**

!!! note "Prompt"
    I am a geospatial consultant working for an agricultural technology company that specialises in precision agriculture. I need to upload drone imagery of a farmer's paddock to John Deere's Operations Centre Map Layers. The drone imagery is a GeoTIFF file. 
    
    Can you write Python code that uploads this image to the correct Map Layers endpoint? 
    
    Can you provide a step-by-step explanation of the process and also review the code for security issues?

    ??? note "Response (Gemini 2.0 Flash)"

        ```python
        import requests
        import json

        # Replace with your actual values
        client_id = "YOUR_CLIENT_ID"
        client_secret = "YOUR_CLIENT_SECRET"
        refresh_token = "YOUR_REFRESH_TOKEN"
        api_url = "https://sandboxapi.deere.com/platform"  # Use sandbox for testing
        tiff_filepath = "path/to/your/drone_imagery.tiff"

        def get_access_token(client_id, client_secret, refresh_token):
            """Retrieves an access token using the provided refresh token."""
            url = f"{api_url}/oauth2/token"
            data = {
                "grant_type": "refresh_token",
                "client_id": client_id,
                "client_secret": client_secret,
                "refresh_token": refresh_token,
            }
            response = requests.post(url, data=data)
            response.raise_for_status()  # Raise an exception for bad status codes
            return response.json()["access_token"]

        def upload_to_map_layers(access_token, tiff_filepath):
            """Uploads a GeoTIFF file to the Map Layers endpoint."""
            url = f"{api_url}/mapLayers"
            headers = {
                "Authorization": f"Bearer {access_token}",
                "Accept": "application/vnd.deere.axiom.v3+json",
            }
            # The 'files' parameter needs to be a list of tuples
            files = [("file", ("drone_imagery.tiff", open(tiff_filepath, "rb"), "image/tiff"))]
            response = requests.post(url, headers=headers, files=files)
            response.raise_for_status()
            return response.json()

        # --- Main execution ---
        access_token = get_access_token(client_id, client_secret, refresh_token)
        response_data = upload_to_map_layers(access_token, tiff_filepath)

        print(json.dumps(response_data, indent=2))  # Print the response
        ```

        Step-by-step explanation
        
        Import necessary libraries:

        `requests`: for making HTTP requests to the API.
        `json`: for handling JSON data.
        
        Set up credentials and API endpoint:

        Replace the placeholder values for `client_id`, `client_secret`, `refresh_token`, and `tiff_filepath` with your actual values.
        
        Use the sandbox API URL (https://sandboxapi.deere.com/platform) for testing. Once your code is working, switch to the production URL.
        
        `get_access_token` function:

        Sends a POST request to the OAuth2 token endpoint to obtain an access token using the refresh token.
        Includes error handling to raise an exception if the request fails.

        `upload_to_map_layers` function:

        * Sends a POST request to the Map Layers endpoint to upload the GeoTIFF file.
        * Sets the necessary headers, including the Authorization header with the access token.
        * Uses the files parameter to include the GeoTIFF file in the request.
        * Includes error handling to raise an exception if the request fails.
        
        Main execution:

        * Calls `get_access_token` to obtain the access token.
        * Calls `upload_to_map_layers` to upload the GeoTIFF file.
        * Prints the JSON response from the API.
        
        Security review
        
        API credentials:

        * Never hardcode API credentials directly in your code. This is a major security risk. Instead, use environment variables, configuration files, or a secrets management service to store and retrieve your credentials.
        * Error handling: The code includes basic error handling to raise exceptions for failed requests.
        Consider adding more specific error handling based on the possible HTTP status codes and error responses from the API.
        * Data validation: The code currently doesn't validate the input data (e.g., tiff_filepath).
        Add validation to ensure the provided file path is valid and the file exists.
        * HTTPS: The code uses HTTPS to communicate with the API, which is essential for secure communication.
        * Sandboxing: Using the sandbox API for testing is a good security practice, as it prevents accidental modifications to production data.
        
        Additional considerations:
        
        * Rate limiting: Be aware of the API's rate limits to avoid exceeding them and getting your requests throttled.
        * Documentation: Refer to the John Deere Operations Center API documentation for details on the Map Layers endpoint and other available functionalities.
        * Dependencies: Ensure you have the required libraries installed (requests). You can install them using `pip install requests`.
        
        Remember to replace the placeholder values with your actual credentials and file path before running the code. Always test your code in the sandbox environment before deploying it to production.

!!! note "Prompt"
    I need to download hourly evaporation data from NASA POWER's API. Can you write a function that will take in date range and location, request the data from NASA POWER's API, and process the response into a pandas DataFrame?

    !!! tip "Practice iterative prompting"
        The initial response provided by ChatGPT is close to a working solution, but has some errors (e.g. the parameter is `EVLAND` not `EVAP`). This illustrates where LLMs can make mistakes and so it is important to verify their output (e.g. run the code). There are also some issues with formatting the date to create the pandas DataFrame. Here, you could do your own research of the NASA POWER API docs to identify the correct parameter and iteratively prompt the LLM to fix the date formatting issue. 

    ??? note "Response (ChatGPT)"
        To download hourly evaporation data from NASA's Prediction Of Worldwide Energy Resources (POWER) API, you can create a Python function that accepts a date range and location, requests the data, and processes the response into a pandas DataFrame. Here's how you can achieve this:

        Step 1: Install Required Libraries

        Ensure you have the necessary libraries installed:
        
        ```bash
        pip install requests pandas
        ```

        Step 2: Define the Function

        Below is the Python function to fetch and process the data:  

        ```python
        import requests
        import pandas as pd

        def fetch_hourly_evaporation_data(start_date, end_date, latitude, longitude):
            """
            Fetch hourly evaporation data from NASA POWER API for a given date range and location.

            Parameters:
            - start_date (str): Start date in 'YYYYMMDD' format.
            - end_date (str): End date in 'YYYYMMDD' format.
            - latitude (float): Latitude of the location.
            - longitude (float): Longitude of the location.

            Returns:
            - pd.DataFrame: DataFrame containing the hourly evaporation data.
            """
            # Define the base URL for the NASA POWER API
            base_url = "https://power.larc.nasa.gov/api/temporal/hourly/point"

            # Define the parameters for the API request
            params = {
                "start": start_date,
                "end": end_date,
                "latitude": latitude,
                "longitude": longitude,
                "parameters": "EVAP",
                "community": "AG",
                "format": "JSON",
                "time-standard": "LST"  # Local Solar Time
            }

            # Make the API request
            response = requests.get(base_url, params=params)
            response.raise_for_status()  # Raise an error for bad status codes

            # Parse the JSON response
            data = response.json()

            # Extract the hourly evaporation data
            records = data.get('properties', {}).get('parameter', {}).get('EVAP', {})

            # Convert the records into a DataFrame
            df = pd.DataFrame.from_dict(records, orient='index', columns=['Evaporation'])
            df.index = pd.to_datetime(df.index)
            df.sort_index(inplace=True)

            return df
        ```   

        Step 3: Usage Example

        Here's how you can use the function:

        ```python
        # Define your parameters
        start_date = "20250101"  # YYYYMMDD
        end_date = "20250107"    # YYYYMMDD
        latitude = -31.9505      # Example: Perth, Australia
        longitude = 115.8605     # Example: Perth, Australia

        # Fetch the data
        evaporation_data = fetch_hourly_evaporation_data(start_date, end_date, latitude, longitude)

        # Display the DataFrame
        print(evaporation_data.head())
        ```

        Explanation:

        API Endpoint and Parameters:
        
        * The function constructs a request to the NASA POWER API's hourly data endpoint for a specific geographic point.
        * Parameters include the date range (start and end), geographic coordinates (latitude and longitude), the desired parameter (EVAP for evaporation), the user community (AG for agriculture), the response format (JSON), and the time standard (LST for Local Solar Time).
        
        Making the Request:
        
        * The function uses the requests library to send a GET request to the API with the specified parameters.
        * It checks for successful response status and raises an error if the request was unsuccessful.
        
        Processing the Response:
        
        * The JSON response is parsed to extract the hourly evaporation data located under properties.parameter.EVAP.
        * This data is converted into a pandas DataFrame, with the timestamps as the index and a column named 'Evaporation'.
        * The index is converted to datetime objects, and the DataFrame is sorted by the index to ensure chronological order.
        
        Note:

        * Ensure that the date range and location provided are within the valid range supported by the NASA POWER API.
        * The API provides data starting from January 1, 2001, to near real-time.
        * For more information on the API and its parameters, refer to the NASA POWER API Documentation.
        
        By following these steps, you can successfully retrieve and process hourly evaporation data for a specified date range and location using the NASA POWER API.

**Use LLMs to help with data visualisation, exploration and communication** 

!!! note "Prompt"
    You are a senior geospatial analyst. You are generating figures for a public government report. The purpose of the report is to highlight within-field yield variability and the opportunities for precision agriculture to increase crop production. 
    
    Can you generate a publication quality figure of an NDVI map for a field with a legend and save it as a PNG file? 
    
    Can you explain the design and style choices when presenting information on the map. 

    !!! tip "Verifying and improving LLM responses"
        The response generated by Gemini 2.0 Flash below is a good example of why it's important to carefully check the LLM response. Overall, it's provided you with Python code that's a good start and its considered lots of elements of a good quality figure (e.g. scale bar and adding coordinates to the map axes). However, there are places where the model's reasoning is erroneous, such as the suggestion to use a red-yellow-green colour palette because it is colour blind friendly (red and green are indistinguishable for certain types of colour blindness). This LLM response is a good reminder for you to consider the accessibility of your figures and choose appropriate colour palettes, and, also, a reminder to check the LLM's reasoning for errors. 

        Also, this is a first attempt. Think about how you could improve the figure (e.g. adding a basemap for reference). This is something you could go back to the LLM with and prompt it to update its response to include basemaps. It is good to get into the habit of treating the LLM as a coding assistant that you bounce ideas and tasks off. 

    ??? note "Response (Gemini 2.0 Flash)"
        I can't directly generate a PNG file and display it here. I'm a text-based AI and don't have the capability to create images. However, I can provide you with the Python code using Matplotlib and a conceptual explanation of how to create such a figure, along with the design considerations for a public government report.  You can then run this code in your own Python environment.

        ```python
        import matplotlib.pyplot as plt
        import numpy as np
        import matplotlib.cm as cm

        # Sample NDVI data (replace with your actual data)
        np.random.seed(42)  # for reproducibility
        rows, cols = 50, 50
        ndvi_data = np.random.uniform(0.2, 0.9, size=(rows, cols))  # Realistic NDVI range

        # Create the figure and axes
        fig, ax = plt.subplots(figsize=(8, 6))  # Adjust figure size as needed

        # Plot the NDVI data as an image
        im = ax.imshow(ndvi_data, cmap=cm.get_cmap('RdYlGn'),  # Use a suitable colormap
                    vmin=0.2, vmax=0.9,  # Set colorbar limits for consistency
                    extent=[0, cols*10, 0, rows*10], # Example: 10m x 10m grid cells
                    origin='lower')  # Ensure correct orientation

        # Add a colorbar with appropriate labels
        cbar = fig.colorbar(im, ax=ax, shrink=0.7)  # Shrink colorbar if needed
        cbar.set_label('Normalized Difference Vegetation Index (NDVI)', fontsize=10)
        cbar.ax.tick_params(labelsize=8)

        # Add a title and axis labels
        ax.set_title('Within-Field NDVI Variability', fontsize=12)
        ax.set_xlabel('Easting (m)', fontsize=10) # Example units
        ax.set_ylabel('Northing (m)', fontsize=10) # Example units
        ax.tick_params(labelsize=8)

        # Add a scale bar (example)
        from mpl_toolkits.axes_grid1.anchored_artists import AnchoredScaleBar
        scalebar = AnchoredScaleBar(ax.transData, 100, "100 m", loc=3, pad=0.5,
                                frameon=False, fontsize=8) # Adjust length and position
        ax.add_artist(scalebar)

        # Improve plot aesthetics
        ax.set_aspect('equal') # Important for geospatial data
        fig.tight_layout()  # Adjust spacing

        # Save the figure as a PNG
        plt.savefig('ndvi_map.png', dpi=300) # High DPI for publication

        plt.show() # If you want to display the plot in your IDE
        ```

        Design and Style Choices Explanation:

        * Colormap:  RdYlGn (Red-Yellow-Green) is a common and intuitive colormap for NDVI, where red indicates lower vegetation health, yellow intermediate, and green higher.  It's also generally colorblind-friendly.  Other options like viridis could be considered.
        * Colorbar Limits: Setting vmin and vmax ensures consistency if you're comparing multiple fields or time periods.  It also prevents the colorbar from being skewed by outliers.  The range 0.2-0.9 is a typical range for NDVI, but you should adjust it based on your data.
        * Figure Size and DPI:  The figsize and dpi settings control the final image resolution.  300 dpi is generally suitable for print publications.
        * Axis Labels and Units: Clear and informative labels are crucial.  Include units (e.g., meters, feet) where appropriate.  If your data is in a specific coordinate system (e.g., UTM), mention it in the figure caption or report text.
        * Title: A concise and descriptive title is essential.
        * Scale Bar: A scale bar provides context and helps the reader understand the spatial dimensions of the field.
        * Aspect Ratio: Setting `ax.set_aspect('equal')` is extremely important for geospatial data. It ensures that the distances on the x and y axes are represented correctly, preventing distortion.
        * Colorbar Label: The colorbar needs a clear label explaining what the colors represent.
        * Font Sizes: Use consistent and readable font sizes for all text elements.
        * Layout: `fig.tight_layout()` helps prevent labels and titles from overlapping.
        * Data:  The code uses np.random.uniform to generate sample NDVI data.  Replace this with your actual NDVI data.  Make sure your data is in a format that Matplotlib can handle (e.g., a 2D NumPy array).
        * Extent: The extent argument in imshow is crucial for correctly positioning your data on the map.  It should correspond to the real-world coordinates of your field.  If you don't provide it, the image will be displayed in pixel coordinates.

        For a government report:

        * Clarity is paramount: Avoid jargon and explain any technical terms.
        * Accuracy is essential: Double-check your data and ensure that the map accurately represents the field conditions.
        * Accessibility: Consider colorblind readers when choosing colormaps.
        * Context: Provide sufficient background information in the report text to explain the significance of the NDVI map and its implications for precision agriculture. Include information about the field, the date of the imagery, and any other relevant details.
        * Caption: The figure should have a clear and concise caption that summarizes the key findings.
        
        Remember to install the necessary libraries: pip install matplotlib numpy.  Replace the sample data with your own and adjust the parameters as needed. This detailed explanation should allow you to create a publication-quality NDVI map for your report.

You can consider using AI to help with more advanced tasks such as creating interactive data visualisations, animations, web pages or dashboards.

!!! note "Prompt"
    I am building a prototype dashboard that will be used to monitor on-farm water resources and crop water requirements. Can you generate an interactive visualisation of a time-series of evaporation data? The evaporation data is stored in a CSV file.

    !!! tip "How could you improve this prompt?"
        This is not a particularly challenging coding task, so it is likely that the model's first response will be pretty close-to-the-mark. However, here are some suggestions for how to improve this prompt:

        * Provide more specifics about the figure you want generated (e.g. colour, themes, styling)
        * Provide more specifics about the figure's interactive features (e.g. what data do you want in hover pop ups?)
        * Be more explicit about the LLMs role (e.g. generate the code using web development best-practice)
        * Encourage the model to think about the task (e.g. you could ask the model to make suggestions on useful interactive features)
        * Ask the LLM to generate a dummy dataset for you to test the code with, this can speed up your development iterations
        * Provide more contextual information (e.g. if this is just a prototype, emphasise that you want a simple codebase that's easy to edit after user testing)

    ??? note "Response (ChatGPT)"
        Below is an example of how you might create an interactive time-series visualization of evaporation data using Plotly and pandas. This example assumes your CSV file has at least two columns (e.g., "Date" and "Evaporation"):

        ```python
        import pandas as pd
        import plotly.express as px

        # Load the evaporation data from CSV
        df = pd.read_csv('evaporation_data.csv')

        # Ensure the 'Date' column is interpreted as datetime
        df['Date'] = pd.to_datetime(df['Date'])

        # Create an interactive line plot for evaporation over time
        fig = px.line(df, x='Date', y='Evaporation', title='Evaporation Time-Series')

        # Add a range slider for interactive zooming and panning
        fig.update_layout(
            xaxis=dict(
                rangeselector=dict(
                    buttons=list([
                        dict(count=1, label="1d", step="day", stepmode="backward"),
                        dict(count=7, label="1w", step="day", stepmode="backward"),
                        dict(count=1, label="1m", step="month", stepmode="backward"),
                        dict(step="all")
                    ])
                ),
                rangeslider=dict(visible=True),
                type="date"
            )
        )

        # Display the interactive plot
        fig.show()
        ```
        Step-by-Step Explanation
        
        Import Libraries:
        
        * pandas is used to load and process the CSV data.
        * plotly.express is used to quickly create interactive plots.
        
        Load the Data:
        
        * The CSV file is read into a pandas DataFrame using pd.read_csv(). Make sure your file path is correct.
        
        Data Preprocessing:
        
        * The "Date" column is converted to a datetime object with pd.to_datetime() to ensure proper time-series handling.
        
        Create the Interactive Plot:
        
        * A line plot is created using px.line(), specifying the x-axis as "Date" and the y-axis as "Evaporation".
        * A title is added to make the chart self-explanatory.
        
        Enhance Interactivity:
        
        * A range selector and range slider are added using fig.update_layout(). This allows users to quickly zoom in on specific periods or pan across the timeline.
        
        Display the Plot:
        
        * Finally, fig.show() renders the interactive visualization in a web browser or an interactive environment (like Jupyter Notebook).
        
        This interactive visualization can be embedded in your prototype dashboard, providing users with an intuitive tool to monitor changes in evaporation over time.


!!! note "Prompt"
    Using best practice web development techniques and open-source packages, create a web dashboard that displays weather data for a location that a user clicks on a web map? When a user clicks on a location add a marker to the map and bring up a popup window that displays current temperature, precipitation, windspeed and humidity. 
    
    Can you provide detailed instructions for how to deploy and test this dashboard?

    !!! tip "Use thinking and reasoning models for complex tasks"
        This is a complex tasks comrpising a series of steps and discrete sub-tasks (e.g. building the frontend web map user interface, code to handle user interactions, building a backend server to make fetch weather data on the fly, detailed instructions for deployment). There are different ways to complete these tasks (e.g. choices between different software packages, design and styling choices and the choices between technical deployment solutions); the model needs to consider these different options and make a decision on the solution when generating a response. The nature of this task (comprising multiple problems and consideration of multiple options) makes it well suited to "thinking" or "reasoning" models. 

        Compare these two responses, one using regular ChatGPT and the other using Gemini 2.0 Flash Thinking with reasoning turned on. ChatGPT without thinking has generated a good first start and a template that you could build on. However, contrast this to the response generated by Gemini 2.0 Flash Thinking which has provided a lot of extra and useful detail:
        
        * detailed instructions on installing software and packages 
        * detailed instructions on how to deploy the dashboard to the cloud/ servers
        * recognised the need for development and production environments
        * justification for suggested software packages and deployment environments
        * suggestions for styling and enhancement
        * suggestions for testing the user interface and backend servers

    ??? note "Respone (ChatGPT)"
        Here’s how to build a web dashboard that displays weather data when a user clicks on a map. The dashboard will use React (Next.js) for the frontend and FastAPI for the backend to fetch weather data from an open-source API like OpenWeatherMap. We'll also use Leaflet.js for the interactive map and Tailwind CSS for styling.

        Project Overview

        When a user clicks on a location:

        ✅ A marker is added to the map.
        ✅ A popup appears with current temperature, precipitation, windspeed, and humidity.
        ✅ Weather data is fetched dynamically from an API.

        Tech Stack

        Frontend: React (Next.js), Leaflet.js, Tailwind CSS
        Backend: FastAPI (Python)
        API: OpenWeatherMap API
        Deployment: Vercel (Frontend) + Railway/Render (Backend)

        Code Implementation

        Step 1: Set Up the Backend (FastAPI)
        
        First, install FastAPI and Uvicorn:

        ```
        pip install fastapi uvicorn requests
        ```

        Create a file backend/main.py:

        ```python
        from fastapi import FastAPI
        import requests
        from fastapi.middleware.cors import CORSMiddleware

        app = FastAPI()

        # Allow frontend to access the backend
        app.add_middleware(
            CORSMiddleware,
            allow_origins=["*"],
            allow_credentials=True,
            allow_methods=["*"],
            allow_headers=["*"],
        )

        # Replace with your OpenWeatherMap API Key
        API_KEY = "YOUR_OPENWEATHERMAP_API_KEY"

        @app.get("/weather/")
        def get_weather(lat: float, lon: float):
            """Fetch current weather data for given lat/lon."""
            url = f"https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&units=metric&appid={API_KEY}"
            response = requests.get(url)
            
            if response.status_code != 200:
                return {"error": "Unable to fetch weather data"}

            data = response.json()
            
            return {
                "temperature": data["main"]["temp"],
                "humidity": data["main"]["humidity"],
                "windspeed": data["wind"]["speed"],
                "precipitation": data["weather"][0]["description"],
            }
        
        ```

        Run the backend server with: `uvicorn main:app --reload`

        ```
        uvicorn main:app --reload
        ```

        The API is now running at http://127.0.0.1:8000/weather/.

        Step 2: Set Up the Frontend (Next.js + Leaflet)

        Create a Next.js project:

        ```js
        npx create-next-app frontend
        cd frontend
        npm install leaflet react-leaflet axios
        Create a file frontend/components/Map.js:

        import { useState } from "react";
        import { MapContainer, TileLayer, Marker, useMapEvents, Popup } from "react-leaflet";
        import axios from "axios";

        const MapComponent = () => {
        const [position, setPosition] = useState(null);
        const [weather, setWeather] = useState(null);

        const fetchWeather = async (lat, lon) => {
            try {
            const response = await axios.get(`http://127.0.0.1:8000/weather/?lat=${lat}&lon=${lon}`);
            setWeather(response.data);
            } catch (error) {
            console.error("Failed to fetch weather data", error);
            }
        };

        function LocationMarker() {
            useMapEvents({
            click(e) {
                const { lat, lng } = e.latlng;
                setPosition([lat, lng]);
                fetchWeather(lat, lng);
            },
            });

            return position ? (
            <Marker position={position}>
                <Popup>
                {weather ? (
                    <>
                    <b>Weather Info</b>
                    <br /> 🌡 Temp: {weather.temperature}°C
                    <br /> 💧 Humidity: {weather.humidity}%
                    <br /> 💨 Wind: {weather.windspeed} m/s
                    <br /> 🌧 Precip: {weather.precipitation}
                    </>
                ) : (
                    "Fetching weather..."
                )}
                </Popup>
            </Marker>
            ) : null;
        }

        return (
            <MapContainer center={[51.505, -0.09]} zoom={5} style={{ height: "500px", width: "100%" }}>
            <TileLayer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" />
            <LocationMarker />
            </MapContainer>
        );
        };

        export default MapComponent;
        ```

        Step 3: Integrate the Map in Next.js

        Modify frontend/pages/index.js:

        ```js
        import dynamic from "next/dynamic";

        const MapComponent = dynamic(() => import("../components/Map"), { ssr: false });

        export default function Home() {
        return (
            <div className="flex flex-col items-center justify-center h-screen">
            <h1 className="text-2xl font-bold mb-4">On-Farm Weather Dashboard</h1>
            <MapComponent />
            </div>
        );
        }
        ```

        4. Running & Testing the Dashboard

        Start the Backend

        ```
        cd backend
        uvicorn main:app --reload
        ```

        Start the Frontend
        
        ```
        cd frontend
        npm run dev
        ```

        Open http://localhost:3000, click anywhere on the map, and see the weather data popup!

        5. Deploying the Dashboard

        Deploy Backend (FastAPI)

        * Use Railway, Render, or Fly.io for FastAPI deployment.
        
        * Example using Railway:
        
        ```
        railway init
        railway up
        ```


        * Update frontend/components/Map.js to use the live API URL.

        Deploy Frontend (Next.js)

        * Deploy to Vercel:
        
        ```
        vercel
        ```

        * Update API endpoint in the frontend with the deployed FastAPI URL.
        
        6. Enhancements & Future Improvements

        ✅ Add weather forecasts for the next few days
        ✅ Store user locations for analysis
        ✅ Integrate satellite imagery for farm monitoring

        This setup ensures best practices in modern web development, leveraging open-source packages for interactivity and real-time data updates. 🚀

    ??? note "Response (Google 2.0 Flash Thinking)"
        Okay, let's create a detailed guide to build your interactive weather dashboard. We'll use best practices by structuring our project, using modern tools, and focusing on maintainability and scalability.

        Project Goal:  A web dashboard with a map. Users click on the map, a marker appears, and a popup displays weather data for that location.

        Technology Stack (Open Source & Best Practice):

        Frontend:

        * React: A popular JavaScript library for building user interfaces. It's component-based, efficient, and well-supported. (Best Practice for modern web UIs)
        * Leaflet: A lightweight, open-source JavaScript library for interactive maps. It's easy to use and has great performance. (Best Practice for map integration)
        * React Leaflet: React components for Leaflet, making integration seamless within React.
        * Axios: A promise-based HTTP client for making API requests from the frontend. (Best Practice for API interaction)
        * CSS Modules or Styled Components: For styling components in a modular and maintainable way. (Best Practice for CSS management)
        
        Backend (Simple API - for this example):

        * Node.js with Express: A minimal and flexible Node.js web application framework. Easy to set up a simple API. (Open Source and good for backend flexibility)
        * OpenWeatherMap API: A widely used weather API (free tier available for development) to fetch weather data. (Open Source data source - requires API key)
        Deployment:

        Netlify or Vercel (Frontend): Easy and free static site hosting for React frontends. (Best Practice for frontend deployment)
        PythonAnywhere, Heroku, or Render (Backend): Platforms for deploying simple backend applications (free or low-cost tiers for initial deployment). (Best Practice for backend deployment)
        Step-by-Step Instructions:

        Phase 1: Project Setup and Backend API (Node.js with Express)

        Set up your Development Environment:

        * Install Node.js and npm (or yarn): Make sure you have Node.js and npm (Node Package Manager) installed on your machine. Download from https://nodejs.org/.
        * Choose a Code Editor: VS Code, Sublime Text, Atom, etc. (VS Code is highly recommended for React development).
        * Create Backend Folder and Initialize Node.js Project:

        Open your terminal and create a folder for your backend (e.g., weather-dashboard-backend):

        ```
        mkdir weather-dashboard-backend
        cd weather-dashboard-backend
        npm init -y  # Initializes a Node.js project with default settings
        ```

        Install Backend Dependencies:

        ```
        npm install express cors dotenv node-fetch
        ```

        * express: For creating the API server.
        * cors: For enabling Cross-Origin Resource Sharing (so your frontend can access the backend).
        * dotenv: To load environment variables (like your API key) from a `.env` file.
        * node-fetch: To make HTTP requests to the OpenWeatherMap API (built-in fetch in newer Node.js versions is also an option)

        Create .env file:

        * In your `weather-dashboard-backend` folder, create a file named `.env`.
        * Sign up for an account at https://openweathermap.org/api and get an API key.
        * Add your API key to the .env file:

        ```
        OPENWEATHERMAP_API_KEY=YOUR_API_KEY_HERE
        ```

        Create server.js (or index.js) for your Express API:

        ```js
        // server.js (or index.js)
        require('dotenv').config(); // Load environment variables from .env
        const express = require('express');
        const cors = require('cors');
        const fetch = require('node-fetch'); // or use built-in fetch

        const app = express();
        const port = process.env.PORT || 5000; // Use PORT env variable if available, otherwise default to 5000

        app.use(cors()); // Enable CORS for all origins (for development - refine for production)

        app.get('/weather', async (req, res) => {
            const { lat, lon } = req.query; // Get latitude and longitude from query parameters

            if (!lat || !lon) {
                return res.status(400).json({ error: 'Latitude and longitude are required.' });
            }

            const apiKey = process.env.OPENWEATHERMAP_API_KEY;
            const apiUrl = `https://api.openweathermap.org/data/2.5/weather?lat=<span class="math-inline">\{lat\}&lon\=</span>{lon}&appid=${apiKey}&units=metric`; // Use metric units

            try {
                const response = await fetch(apiUrl);
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const weatherData = await response.json();
                res.json(weatherData);
            } catch (error) {
                console.error('Error fetching weather data:', error);
                res.status(500).json({ error: 'Failed to fetch weather data.' });
            }
        });

        app.listen(port, () => {
            console.log(`Server listening on port ${port}`);
        });
        ```

        Run the Backend API:

        ```
        node server.js  # or node index.js if you named it index.js
        ```

        Your backend API should be running on http://localhost:5000.

        Test the Backend API:

        * Open a browser or use a tool like Postman/curl.
        * Access the endpoint with latitude and longitude parameters, e.g.,: http://localhost:5000/weather?lat=51.5074&lon=0.1278 (London coordinates)
        * You should see a JSON response with weather data from OpenWeatherMap.
        
        Phase 2: Frontend Setup (React) and Map Integration

        Create Frontend Folder and React App:

        In the same parent directory as your `weather-dashboard-backend` folder, create a frontend folder (e.g., `weather-dashboard-frontend`):

        ```
        npx create-react-app weather-dashboard-frontend
        cd weather-dashboard-frontend
        ```

        This uses create-react-app (CRA), a best practice for quickly setting up a React project.

        Install Frontend Dependencies:

        ```
        npm install leaflet react-leaflet axios
        ```

        * leaflet: Leaflet library.
        * react-leaflet: React components for Leaflet.
        * axios: For making API requests to your backend.
        
        Clean up src folder:

        * Delete unnecessary files from src like `App.css`, `logo.svg`, `reportWebVitals.js`, `setupTests.js`.
        * Modify `index.js` to just render App component.
        * Modify `App.js` (Basic Map Component):

        ```js
        // src/App.js
        import React, { useState } from 'react';
        import { MapContainer, TileLayer, Marker, Popup } from 'react-leaflet';
        import 'leaflet/dist/leaflet.css'; // Import Leaflet CSS
        import axios from 'axios';
        import L from 'leaflet'; // Import Leaflet for icon fix

        // Fix for Leaflet marker icon issue (if needed - sometimes CRA and Leaflet conflict)
        delete L.Icon.Default.prototype._getIconUrl;
        L.Icon.Default.mergeOptions({
            iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
            iconUrl: require('leaflet/dist/images/marker-icon.png'),
            shadowUrl: require('leaflet/dist/images/marker-shadow.png')
        });


        function App() {
            const [markerPosition, setMarkerPosition] = useState(null);
            const [weatherData, setWeatherData] = useState(null);

            const handleMapClick = async (event) => {
                const { lat, lng } = event.latlng;
                setMarkerPosition({ lat, lng });

                try {
                    const response = await axios.get('http://localhost:5000/weather', { // Adjust backend URL if needed
                        params: { lat, lon: lng }
                    });
                    setWeatherData(response.data);
                } catch (error) {
                    console.error('Error fetching weather data:', error);
                    setWeatherData({ error: 'Failed to fetch weather data' }); // Set an error state
                }
            };

            return (
                <div style={{ height: '100vh', width: '100%' }}>
                    <MapContainer center={[51.505, -0.09]} zoom={13} style={{ height: '100%', width: '100%' }} onClick={handleMapClick}>
                        <TileLayer
                            url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
                            attribution='&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
                        />
                        {markerPosition && (
                            <Marker position={[markerPosition.lat, markerPosition.lng]}>
                                <Popup>
                                    {weatherData ? (
                                        weatherData.error ? (
                                            <p>Error: {weatherData.error}</p>
                                        ) : (
                                            <div>
                                                <p>Temperature: {weatherData.main?.temp}°C</p>
                                                <p>Description: {weatherData.weather?.[0]?.description}</p>
                                                <p>Wind Speed: {weatherData.wind?.speed} m/s</p>
                                                <p>Humidity: {weatherData.main?.humidity}%</p>
                                                {/* Add more weather data as needed */}
                                            </div>
                                        )
                                    ) : (
                                        <p>Loading weather data...</p>
                                    )}
                                </Popup>
                            </Marker>
                        )}
                    </MapContainer>
                </div>
            );
        }

        export default App;
        ```
        * Your frontend app should be running on http://localhost:3000.

        Test the Dashboard:

        * Open http://localhost:3000 in your browser.   
        * Click on the map.
        * A marker should appear, and after a moment, a popup should display weather data for that location.
        * If you see "Loading weather data..." and it doesn't resolve, check your backend API is running correctly, and your API key is valid.   
        * Inspect the browser's developer console (F12) for any errors (Network, Console tabs).
        
        Phase 3: Styling and Enhancements (Optional but Recommended Best Practices)

        Styling:   

        * CSS Modules: In `App.js`, you can create `App.module.css` file for component-specific styling and import it: `import styles from './App.module.css';` and use class names like `className={styles.mapContainer}`.   
        * Styled Components: Install `npm install styled-components` and use them to create styled components within your React code.
        * Tailwind CSS: Install `npm install -D tailwindcss postcss autoprefixer` and configure Tailwind CSS for rapid utility-first styling.
        
        Enhancements (Best Practices):

        * Error Handling: Improve error messages in popups.
        * Loading States: Implement better loading indicators (spinners).
        * UI/UX Improvements: Design a better popup, maybe use icons for weather conditions, make the map responsive.
        * Caching: Implement client-side or server-side caching to reduce API calls for frequently clicked locations.
        * Input Field for Location Search: Add a search bar using a geocoding service (like OpenStreetMap Nominatim or Google Maps Geocoding API) to allow users to search for locations by name and then click on the map.
        * More Weather Data: Display more relevant weather information from the OpenWeatherMap API response (sunrise/sunset, feels like temperature, pressure, etc.).
        * Weather Icons: Use weather condition codes from OpenWeatherMap to display relevant weather icons in the popup.
        * Responsive Design: Ensure the dashboard works well on different screen sizes (mobile, tablet, desktop).
        
        Phase 4: Deployment and Testing

        Deployment - Frontend (React - using Netlify as an example):

        Build your React Frontend:

        ```
        cd weather-dashboard-frontend
        npm run build
        ```

        This creates an optimized production build of your React app in the build folder.

        Deploy to Netlify:

        * Sign up for a free Netlify account at https://www.netlify.com/.
        * Install Netlify CLI globally (optional but recommended for more control): npm install netlify-cli -g.
        * From your weather-dashboard-frontend directory:

        ```
        netlify deploy --prod
        ```

        (If you don't have Netlify CLI, you can also drag and drop the `build` folder to the Netlify web interface.)   
        
        * Netlify will give you a live URL for your frontend.
        
        Deployment - Backend (Node.js/Express - using PythonAnywhere as an example):

        Prepare for Deployment:

        * Procfile: Create a `Procfile` in your `weather-dashboard-backend` folder (no file extension):

        ```
        web: node server.js
        ```
        * package.json (engines): Add an engines section to your package.json to specify the Node.js version:
        
        ```
        "engines": {
            "node": "18.x"  // Or a specific Node.js version you used
        },
        ```

        Deploy to PythonAnywhere (Free Tier - suitable for testing, limitations apply):

        * Sign up for a free PythonAnywhere account at https://www.pythonanywhere.com/.
        * Create a Web App: Go to the "Web" tab and click "Add a new web app." Choose "Manual configuration" and Node.js.
        * Upload your Backend Code: Use the "Files" tab to upload the contents of your weather-dashboard-backend folder (including `package.json`, `server.js`, `Procfile`, `.env`). *Important: On PythonAnywhere free tier, you may need to manually set environment variables within your web app configuration instead of using a .env file if file access restrictions are in place.*   
        * Install Dependencies: In a PythonAnywhere console (bash), navigate to your backend directory and run npm install.
        * Configure Web App Settings: In the "Web" tab, ensure your "Working directory" points to your backend directory and that the "WSGI file" (if prompted) is correctly configured (it usually defaults to a basic setup). Make sure to point the "webapp URL" to your `server.js` or `index.js` as specified in your `Procfile`.

        Deployment - Alternatives:

        * Frontend: Vercel (similar to Netlify, also excellent for React), AWS S3, Google Firebase Hosting, GitHub Pages.   
        * Backend: Heroku (more robust, often easier for Node.js), Render (another good PaaS), AWS EC2/Elastic Beanstalk, Google Cloud App Engine, DigitalOcean.
        
        Testing - Detailed Instructions:

        Local Development Testing:

        * Frontend: Test at http://localhost:3000 while running npm start in your frontend.
        * Backend: Test at http://localhost:5000 (and specific API endpoints) while running `node server.js` in your backend.
        * Use browser developer tools (F12 -> Network tab, Console tab) to inspect API requests, responses, and any errors.
        * Test clicking on different map locations.
        
        Deployed Frontend Testing (on Netlify/Vercel):

        * Access your live Netlify/Vercel URL.
        * Repeat map clicking tests.
        * Check if the frontend is correctly fetching data from your deployed backend (the URL in axios.get in your frontend needs to point to your deployed backend URL, not localhost:5000 anymore - see "Phase 5: Configuration for Deployment" below).
        
        Deployed Backend Testing (on PythonAnywhere/Heroku/Render):

        * Access your deployed backend URL directly (e.g., from PythonAnywhere's web app URL).
        * Test the `/weather` endpoint with lat/lon parameters (e.g., `your-pythonanywhere-url.pythonanywhere.com/weather?lat=40.7128&lon=-74.0060`).
        * Ensure it returns valid weather data.
        
        Cross-Browser Testing:

        Test your deployed dashboard on different browsers (Chrome, Firefox, Safari, Edge) and different devices (desktop, mobile, tablet) to ensure compatibility and responsiveness.
        Performance Testing (Basic):

        * Use browser developer tools (Network tab -> check "Disable cache" and reload) to measure API request times.
        * If you have caching implemented, test with and without cache to see performance improvements.
        
        Error Handling Testing:

        * Simulate API errors:
        * Intentionally use an invalid API key in your backend `.env`.
        * Temporarily take down your backend server.
        * Test what happens in the frontend when the API call fails – error messages in popups, graceful handling.
        
        User Acceptance Testing (UAT):

        Ask other people to use your dashboard and give feedback on usability, functionality, and any issues they encounter.
        
        Phase 5: Configuration for Deployment (Important)

        Backend URL in Frontend:  When you deploy, your frontend will need to connect to your deployed backend URL, not http://localhost:5000.

        Environment Variables in Frontend: Best practice is to use environment variables in your React frontend as well. Create `.env.production` and `.env.development` files in your frontend root.
        
        In `.env.development`:

        ```
        REACT_APP_BACKEND_URL=http://localhost:5000
        ```

        In `.env.production`:

        ```
        REACT_APP_BACKEND_URL=YOUR_DEPLOYED_BACKEND_URL_HERE  // Replace with your actual deployed backend URL
        ```

        * Access the URL in your React code like this: `process.env.REACT_APP_BACKEND_URL`.
        * Netlify/Vercel Environment Variables: When deploying to Netlify or Vercel, you can also set environment variables in their platform settings, which is a more secure way for production.
        * CORS for Production: For a production setup, you should restrict CORS on your backend to only allow requests from your deployed frontend URL instead of `cors()`. You can configure cors middleware to allow specific origins.

        Important Considerations:

        * API Rate Limits: Be mindful of the OpenWeatherMap API free tier rate limits. For more usage, you might need to upgrade to a paid plan or implement caching.
        * Security: For a production application, you'd need to think more about security:
        * Securely manage API keys (environment variables, don't expose them in frontend code).
        * HTTPS for all communication.
        * Input validation and sanitization.
        * Consider authentication and authorization if you need to control access.
        * Scalability: For a highly scalable application, you might choose different backend technologies (like serverless functions or more robust cloud infrastructure) and database solutions.
        
        This detailed guide should give you a solid foundation for building and deploying your interactive weather dashboard. Remember to test thoroughly at each stage and iterate based on feedback! Good luck!

**AI code editors**

There are also AI enabled code editors which let you access AI support in the text editor environment where you write code. These tools have a mix of free and paid options:

* <a href="https://code.visualstudio.com/docs/copilot/overview" target="_blank">copilot in Visual Studio Code</a>
* <a href="https://www.cursor.com" target="_blank">Cursor AI</a>
* <a href="https://codeium.com/windsurf" target="_blank">Windsurf</a>

## AI for literature reviews, studying, thinking and writing

Before using AI tools to work with literature, it’s important to be aware of the content's terms of service and the copyright, licensing and privacy implications. You should research the copyright and licensing of any material you upload to and use with AI literature tools. 

!!! tip "General guidelines for uploading data to AI tools"
    * Check how the AI service provider uses your data (i.e. content you upload and your prompts). If possible, select options and use tools that are not trained on and do not view your data. 
    * Do not share content that you generate with AI tools. Use these tools privately to enhance your studying, thinking and understanding. Rather than using AI tools as text summarisers / generators, use them to be more active in how you engage with literature and as aids to help you practice critical and analytical reading and thinking. 

**I want to practice critical / analytical reading and thinking** 

Look at how the AI tools identify limitations and gaps with existing studies as a template for your own reading AND critique the AI responses too - do you agree with the limitations and gaps the AI has identified?

* Use  <a href="https://storm.genie.stanford.edu" target="_blank">STORM Genie's</a> CO-STORM feature to generate a round table discussion of a topic. This will generate a discussion of a topic presenting different views and perspectives.

!!! tip "Review STORM and CO-STORM's brainSTORMING process"
    When generating Wikipedia-like articles or round table discussions STORM undertakes a "brainstorming" process where agents from different perspectives ask and answer questions on a topic, update their knowledge and generate new ideas. Looking at this trace is a good way for you to practice approaching a topic from different perspectives and also identify effective questions that you can reuse elsewhere when researching topics and undertaking literature reviews. 

    <iframe src="https://drive.google.com/file/d/1XnY5QiJw50akczvn7tedEQXFkiVxLKur/preview" width="640" height="480" allow="autoplay"></iframe>

    *The STORM interface and viewing the brainSTORMING process.*

* Upload a paper to <a href="https://notebooklm.google.com" target="_blank">NotebookLM</a> and prompt it to generate a list of limitations and recommendations for further work. Or, ask <a href="https://storm.genie.stanford.edu" target="_blank">STORM Genie</a> to create to review the literature and create a list of limitations with the current state of the literature for a topic area or task.

!!! tip "Reviewing lists of knowledge gaps or recommendations for further research"
    Actively engage with the lists of limitations and recommendations by prioritising them for further study by considering criteria of impact (what benefits will accrue if the limitation is addressed) and feasibility (what is holding back progress in this area? data availability? knowledge? financial resources?).

    Also, consider if agree with the limitations and gaps the AI has identified? Are there gaps that you think the AI has overlooked? Why might that be the case?

* Ask <a href="https://blog.google/products/gemini/google-gemini-deep-research/" target="_blank">Google Deep Research (costs)</a> or <a href="https://www.perplexity.ai" target="_blank">Perplexity (Deep Research)</a>  to provide a review of knowledge gaps or methodological limitations for a topic area. When reading a paper, consider how it addresses these gaps and identify what its contribution to the literature is. Also, reflect on how relevant the gaps and limitations generated by the AI are. 

**I want to create a background overview of a subject area before reading papers in detail**

Generating a literature review prior to reading can help you situate the paper within the broader literature, give you tips to help develop your critical reading and provide you with a background understanding of relevant concepts and theory, which will make your reading more efficient and effective.

Ask <a href="https://storm.genie.stanford.edu" target="_blank">STORM</a>, <a href="https://www.perplexity.ai" target="_blank">Perplexity (Deep Research)</a>, or  <a href="https://blog.google/products/gemini/google-gemini-deep-research/" target="_blank">Google Deep Research (costs)</a> to prepare a subject review (follow up, check and add to the sources used by the AI tool):

!!! note "Prompt"
    Can you write a literature review on land cover classification methods using remote sensing images?

!!! note "Prompt"
    Can you write a literature review on harvest weed seed control?

!!! note "Prompt"
    Can you write a review on Western Australian drylands salinity challenges and management practices?

!!! note "Prompt"
    Can you write a review of methods to segment field boundaries from satellite images? Can you focus on approaches most suited to smallholder cropping systems?

!!! note "Prompt"
    Can you write a review on methods to classify crop type from remote sensing images? 

**I want to create an overview of organisation’s resources / policies**

Ask <a href="https://blog.google/products/gemini/google-gemini-deep-research/" target="_blank">Google Deep Research (costs)</a>, <a href="https://www.perplexity.ai" target="_blank">Perplexity (Deep Research or Pro Search)</a> or <a href="https://chatgpt.com" target="_blank">ChatGPT (with Search and Reason turned on)</a> to complete a web search and generate a report (follow up, check and add to the sources used by the AI tool):

!!! note "Prompt"
    Can you write a review of the Western Australian Government’s Department of Primary Industry and Regional Development’s recommendations for constructing farm dams?

**I have 10 minutes and want a quick overview of a paper**

* Upload the paper to <a href="https://notebooklm.google.com" target="_blank">NotebookLM</a> and ask it to create a short podcast conversation about the paper
* Upload the paper to <a href="https://notebooklm.google.com" target="_blank">NotebookLM</a> and ask it to generate a study guide / briefing document
* Upload the paper to <a href="https://notebooklm.google.com" target="_blank">NotebookLM</a> and ask it to generate a blog-style review of the paper

**I want to test my understanding of a paper / topic**

* Upload the paper to <a href="https://notebooklm.google.com" target="_blank">NotebookLM</a> and prompt it to generate a study quiz for you
* Upload the paper to <a href="https://notebooklm.google.com" target="_blank">NotebookLM</a> and ask it to generate text for a set of Anki cards with answers

**I’m struggling to understand an equation / figure / section of a paper**

* Screen share with <a href="https://aistudio.google.com" target="_blank">Google AI Studio</a> and verbally ask for an explanation of the content that you’re stuck with. Keep prompting until you are satisfied with your understanding.
