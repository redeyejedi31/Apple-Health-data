# Apple Health Data Export Structure

This document provides an unofficial guide to the XML structure used in Apple Health data exports. Apple Health exports are massive files that contain detailed information about your health metrics, fitness activities, and daily activity summaries.

Please note that the XML sample provided below is generated to demonstrate the structure and is not a full representation of your complete data.

## Root Element: &lt;HealthData&gt;
The root container for the entire export.
* **locale**: The geographic locale of the device at the time of export.

## Primary Elements

### 1. &lt;ExportDate&gt;
Indicates the exact time the export was performed.
* **value**: Timestamp of the export (e.g., YYYY-MM-DD HH:MM:SS +Offset).

### 2. &lt;Me&gt;
Contains static personal information stored in your Health profile.
* **dateOfBirth**: Your date of birth.
* **biologicalSex**: The biological sex recorded in your profile.
* **bloodType**: Your blood type, if configured.
* **fitzpatrickSkinType**: Skin type categorization used for UV index processing.
* **handPreference**: Hand preference setting.

### 3. &lt;Record&gt;
Represents granular health metrics. This is the most frequent element type. Note that for continuous measurements like heart rate, the system typically records samples at dynamic intervals, which can vary based on your activity state and device settings (for example, higher frequency during workouts compared to rest periods).
* **type**: The specific measurement identifier (e.g., HKQuantityTypeIdentifierHeartRate).
* **sourceName**: The device or app that recorded the data.
* **sourceVersion**: Software version of the source.
* **device**: Hardware details of the recording device.
* **unit**: Unit of measurement (e.g., count/min, kcal, km).
* **creationDate / startDate / endDate**: Timestamps for the record.
* **value**: The recorded measurement value.
* **MetadataEntry**: Nested diagnostic details.
  * **key**: The metadata category (e.g., HKMetadataKeyHeartRateMotionContext).
  * **value**: The diagnostic value.

### 4. &lt;Workout&gt;
Represents a fitness activity session. During these sessions, the system captures various metrics such as heart rate, pace, or swimming distance at varying intervals depending on the activity intensity and sensor availability.
* **workoutActivityType**: The activity category (e.g., HKWorkoutActivityTypeWalking).
* **duration / durationUnit**: Total session time.
* **totalDistance / totalDistanceUnit**: Total distance covered.
* **totalEnergyBurned / totalEnergyBurnedUnit**: Total active energy expended.
* **creationDate / startDate / endDate**: Timestamps for the workout.
* **WorkoutEvent**: Nested event markers.
  * **type**: Event type (e.g., HKWorkoutEventTypePause).
  * **date**: Timestamp of the event.
  * **duration**: Duration of this specific event.

### 5. &lt;ActivitySummary&gt;
A daily summary of your activity rings.
* **dateComponents**: The date of the summary.
* **activeEnergyBurned / activeEnergyBurnedGoal**: Daily active energy progress.
* **activeEnergyBurnedUnit**: Unit for energy (e.g., kcal).
* **appleExerciseTime / appleExerciseTimeGoal**: Daily exercise minutes.
* **appleStandHours / appleStandHoursGoal**: Daily stand hours.

## XML Sample
```xml
<?xml version="1.0" encoding="UTF-8"?>
<HealthData locale="en_GB">
  <ExportDate value="2026-06-25 15:30:00 +0100"/>
  
  <Me dateOfBirth="1990-01-01" 
      biologicalSex="HKBiologicalSexNotSet" 
      bloodType="HKBloodTypeNotSet" 
      fitzpatrickSkinType="HKFitzpatrickSkinTypeNotSet" 
      handPreference="HKHandPreferenceNotSet"/>

  <Record type="HKQuantityTypeIdentifierHeartRate" 
          sourceName="Apple Watch" 
          sourceVersion="9.0.2" 
          device="<<HKDevice: 0x790bbdec0>, name:Apple Watch, manufacturer:Apple Inc., model:Watch, hardware:Watch6,4, software:9.0.2, creation date:2022-10-12 09:15:00 +0000>" 
          unit="count/min" 
          creationDate="2026-06-25 09:15:00 +0100" 
          startDate="2026-06-25 09:15:00 +0100" 
          endDate="2026-06-25 09:15:00 +0100" 
          value="89">
    <MetadataEntry key="HKMetadataKeyHeartRateMotionContext" value="1"/>
  </Record>

  <Workout workoutActivityType="HKWorkoutActivityTypeWalking" 
           duration="30.5" 
           durationUnit="min" 
           totalDistance="2.5" 
           totalDistanceUnit="km" 
           totalEnergyBurned="150.0" 
           totalEnergyBurnedUnit="kcal" 
           creationDate="2026-06-25 10:30:00 +0100" 
           startDate="2026-06-25 10:00:00 +0100" 
           endDate="2026-06-25 10:30:00 +0100">
    <WorkoutEvent type="HKWorkoutEventTypePause" 
                  date="2026-06-25 10:15:00 +0100" 
                  duration="5.0"/>
  </Workout>

  <ActivitySummary dateComponents="2026-06-25" 
                   activeEnergyBurned="400.0" 
                   activeEnergyBurnedGoal="500.0" 
                   activeEnergyBurnedUnit="kcal" 
                   appleExerciseTime="30.0" 
                   appleExerciseTimeGoal="30.0" 
                   appleStandHours="10.0" 
                   appleStandHoursGoal="12.0"/>
</HealthData>
