---
layout: default
title: track desk time widget
---
I wanted to be able to use the data from my basement desk-light to calculate how many hours I work a week. The idea was that I just needed a way to accumulate all the time the light spent in the `on` state and visualize that in a widget. Then at the end of the week, the time would reset to zero.

## Part 1: The Input Sensor (Tracking On/Off State)
This sensor is the foundation; it outputs a numerical value of 1 when your desk light is on and 0 when it's off.

Navigate to Helpers: Go to Settings → Devices & Services → Helpers.

Create Template Sensor: Click + CREATE HELPER and select Template Sensor.

Configuration:

Name: Desk Light State (e.g., sensor.desk_light_state)

State template: Paste the following code, replacing the placeholder with your smart plug's entity ID (e.g., switch.basement_desk_light_smart_plug).

{% raw %}
```
{% if is_state('switch.basement_desk_light_smart_plug', 'on') %}
  1
{% else %}
  0
{% endif %}
Unit of measurement: s (seconds)
```
{% endraw %}

State class: Leave this field BLANK (This is the critical fix for the overcounting issue).

Device class: duration

Action: Click CREATE.

## Part 2: The Accumulator (Summing the Time)
This helper sums the seconds from the input sensor using the correct integration method.

Install Integration Helper: Ensure the Integration integration is installed (if you don't see the helper).

Create Integration Helper: Go to Helpers, click + CREATE HELPER, and select Integration - Riemann sum integral.

Configuration:

Name: Desk Time Accumulator (e.g., sensor.desk_time_accumulator)

Input entity: Select your new Template Sensor (e.g., sensor.desk_light_state).

Method: Select Left Riemann sum.

Action: Click CREATE.

## Part 3: The Reset Mechanism (Daily Total)
This helper tracks the accumulator and automatically resets the count every day.

Create Utility Meter: Go to Helpers, click + CREATE HELPER, and select Utility Meter.

Configuration:

Name: Daily Desk Time (e.g., sensor.daily_desk_time)

Input entity: Select the Integration sensor from the previous step (e.g., sensor.desk_time_accumulator).

Meter resets: Select Daily.

Action: Click CREATE.

## Part 4: The Final Visualization (Readable Time)
This final helper converts the total seconds from the utility meter into an easy-to-read format (e.g., "1h 32m").

Create Template Sensor: Go to Helpers, click + CREATE HELPER, and select Template Sensor.

Configuration:

Name: Daily Desk Time Readable

State template: Paste the code below, ensuring you use the entity ID of your Utility Meter (from Part 3).

{% raw %}
```
{% set seconds = states('sensor.daily_desk_time') | float(0) %}
{% set hours = (seconds // 3600) | int %}
{% set minutes = ((seconds % 3600) // 60) | int %}
{% set remaining_seconds = (seconds % 60) | int %}

{% if hours > 0 %}
  {{ hours }}h {{ minutes }}m
{% elif minutes > 0 %}
  {{ minutes }}m {{ remaining_seconds }}s
{% else %}
  {{ remaining_seconds }}s
{% endif %}
Unit of measurement: (Leave blank for a clean display)
```
{% endraw %}

Action: Click CREATE.

## Final Step: Restart Home Assistant
For all changes to take effect and for the Integration helper to correctly read the fixed unit and state class, you must restart Home Assistant. Go to Settings → System → Restart Home Assistant.

Use the final sensor.daily_desk_time_readable entity on your dashboard for the final display