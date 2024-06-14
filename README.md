# Rebate System: The Complete Guide

## Table of Contents
2. [Database Schema Design](#database-schema-design)
3. [Backend Logic](#backend-logic)
## Introduction

The "Rebate System" refers to a cashback mechanism where a certain percentage of the valid bets of a player over a specific period in a particular game or game category is returned to the player later. This practice is typically used to attract and retain customers.

## Database Schema Design

Define the necessary database tables to store rebate information, player data, and transaction history.

### Tables:

1. **rebates**
   - `id` (primary key)
   - `title` (string)
   - `description` (text)
   - `category` (string)
   - `start_date` (date)
   - `end_date` (date)
   - `rebate_cycle` (enum: daily, weekly, monthly)
   - `status` (enum: active, inactive)
   - `updated_by` (string)
   - `updated_at` (timestamp)
   - `cycle_timezone` (string)
   - `max_group_rebate` (decimal)

3. **rebate_conditions_table**
   - `id` (primary key)
   - `rebate_id` (foreign key)
   - `game_provider_id` (foreign key) 
   - `category_id` (foreign key)(optional)
   - `valid_turnover` (decimal)
   - `rebate_percentage` (decimal)
   - `max_rebate_amount` (decimal)
   - `condition_multiplier` (integer)

4. **player_balance_history**
    ### Need to add this columns in : player_balance_history table
   - `rebate_id` (foreign key)
   - `record_type` (string: rebate string:Normal)

## Backend Logic

### Steps:

1. **Rebate Creation API**
   - Create an API endpoint for creating rebates.
   - Validate input data.
   - Store the rebate details in the `rebates` table.
   - Store each condition in the `rebate_conditions` table.

2. **Cron Job for Rebate Calculation**
   - Set up a cron job that runs at 00:00:00 based on the `cycle_timezone` and `rebate_cycle`.
   - Fetch all active rebates.
   - For each rebate, check player turnover against the conditions.
   - Calculate the rebate amount based on the percentage and give logic as per SOP of Rebate
   - Update player balance and create a new record in `player_balance_history`.

3. **Rebate Activation/Deactivation/Delete**
   - Create an API endpoint to update the rebate status to active or inactive or draft.
   - Ensure that in delete operation is allowed, only when status is draft .
