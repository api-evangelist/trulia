# Trulia GraphQL Schema

## Overview

This is a conceptual GraphQL schema for Trulia, the U.S. home and rental search portal owned by Zillow Group. Trulia's historical public REST APIs (Property, Stats, Schools, Locations) were deprecated after the 2015 Zillow Group acquisition. This schema models the data surfaces Trulia exposed — listings, property details, neighborhood statistics, school ratings, transit and walkability scores, crime grades, flood risk, home value trends, and rental indexes — as a modern GraphQL API.

Equivalent live data is now accessible only through Zillow Group partner programs (Bridge Interactive, Zillow Tech Connect, HotPads).

## Schema Source

Conceptual — derived from Trulia's historical public API documentation, the Trulia developer portal (now offline), and Zillow Group successor surfaces.

## Root Types

### Query

- `property(id: ID!)` — fetch a single property by Trulia property ID
- `listingsForSale(filter: ListingFilter, pagination: PaginationInput)` — search active for-sale listings
- `listingsForRent(filter: RentalFilter, pagination: PaginationInput)` — search active rental listings
- `neighborhood(id: ID!)` — fetch neighborhood details and statistics
- `neighborhoodByLocation(lat: Float!, lng: Float!)` — resolve neighborhood from coordinates
- `city(name: String!, stateCode: String!)` — fetch city-level data
- `zipCode(code: String!)` — fetch ZIP code-level data
- `school(id: ID!)` — fetch a single school record
- `schoolsNearProperty(propertyId: ID!, radius: Float)` — list schools near a property
- `agent(id: ID!)` — fetch an agent profile
- `openHouses(filter: OpenHouseFilter)` — list upcoming open houses
- `mortgageRates(loanType: LoanType, term: MortgageTerm)` — current mortgage rate estimates
- `homeValues(locationId: ID!, locationType: LocationType!)` — home value index for an area
- `rentIndex(locationId: ID!, locationType: LocationType!)` — rent index for an area
- `walkScore(propertyId: ID!)` — walk/transit/bike scores for a property

## Named Types (55+)

| Type | Description |
|------|-------------|
| `Property` | Core property node with address, type, details, scores, and listing link |
| `ListingForSale` | Active or historic for-sale listing with price, status, agent |
| `ListingForRent` | Active rental listing with rent range and unit availability |
| `PropertyDetails` | Beds, baths, sqft, lot size, year built, parking, HOA |
| `Address` | Street, unit, city, state, ZIP, county, lat/lng |
| `City` | City name, state, population, median home value |
| `State` | State name, abbreviation, FIPS code |
| `ZipCode` | ZIP code, primary city, county, lat/lng centroid |
| `County` | County name, state, FIPS code |
| `Neighborhood` | Neighborhood name, city, boundary polygon, stats |
| `NeighborhoodDetails` | Extended neighborhood profile including amenity counts |
| `NeighborhoodStats` | Median list price, median rent, days on market, inventory |
| `School` | School name, type, district, rating, address |
| `SchoolDetails` | Enrollment, grades served, student-teacher ratio, test scores |
| `SchoolRating` | Overall rating and sub-ratings (math, reading, equity) |
| `SchoolType` | Enum: ELEMENTARY, MIDDLE, HIGH, K8, K12, CHARTER, PRIVATE |
| `SchoolDistrict` | District name, state, number of schools |
| `PropertyType` | Enum: SINGLE_FAMILY, CONDO, TOWNHOME, MULTI_FAMILY, LOT, MOBILE_HOME, APARTMENT |
| `SingleFamily` | Single-family specific attributes (garage, pool, basement) |
| `Condo` | Condo-specific attributes (floor, HOA fees, building amenities) |
| `Townhome` | Townhome-specific attributes (stories, attached/detached) |
| `MultiFamily` | Multi-family attributes (unit count, cap rate, gross yield) |
| `Lot` | Vacant lot attributes (acreage, zoning, utilities) |
| `MobileHome` | Mobile/manufactured home attributes (park name, pad rent) |
| `Apartment` | Apartment complex with units, amenities, rental details |
| `Rental` | Rental record linking a unit and its listing |
| `RentalDetails` | Lease term, deposit, pet policy, utilities included |
| `UnitAvailability` | Unit type, rent, sqft, available date, availability status |
| `RentRange` | Minimum and maximum rent for a unit type or area |
| `IncomeRestricted` | Flag and AMI percentage for income-restricted units |
| `ApartmentAmenity` | In-unit or building amenity (washer/dryer, gym, pool) |
| `NeighborhoodAmenity` | Nearby POI category and count within a radius |
| `Restaurant` | Restaurant name, cuisine, rating, distance from property |
| `GroceryStore` | Grocery store name, chain, distance from property |
| `CoffeeShop` | Coffee shop name, brand, distance from property |
| `WalkScore` | Walk Score value (0–100) and description |
| `TransitScore` | Transit Score value and nearby transit lines |
| `BikeScore` | Bike Score value and bike infrastructure notes |
| `CrimeGrade` | Overall crime grade (A–F) and sub-grades by crime type |
| `PollutionLevel` | Air quality index, superfund proximity, industrial proximity |
| `FloodRisk` | FEMA flood zone, risk level, estimated annual loss |
| `HomeValues` | Median home value, YoY change, percentile distribution |
| `RentIndex` | Median rent, YoY change, rent-to-income ratio |
| `TrendData` | Time-series data point with date and numeric value |
| `HistoricalPrice` | Historical list/sale price with date and event type |
| `Price` | Amount and currency code |
| `RentTrend` | Monthly rent trend data point for a location |
| `Agent` | Agent name, license, brokerage, contact info |
| `AgentDetails` | Bio, years of experience, languages, specialties |
| `AgentReviews` | Aggregate rating and list of individual reviews |
| `OpenHouse` | Open house event with property, date, time window, host |
| `ShowingRequest` | Buyer-initiated showing request with preferred times |
| `MortgageRate` | Rate type, APR, term, points for a loan product |
| `MortgageCalculator` | Input/output for monthly payment estimates |
| `APIKey` | API key record (historical — no new keys issued) |
| `Token` | OAuth/session token for partner access |
| `PaginationInput` | Page size and cursor for paginated queries |
| `ListingFilter` | Filter inputs for for-sale listing search |
| `RentalFilter` | Filter inputs for rental listing search |
| `OpenHouseFilter` | Filter inputs for open house search |

## Notes

- Trulia's original APIs were XML-first. This schema translates those surfaces into idiomatic GraphQL.
- The `WalkScore`, `TransitScore`, and `BikeScore` types reflect data Trulia licensed from Walk Score (now also part of Redfin/RealPage ecosystem).
- Crime, flood, and pollution data were key Trulia differentiators over competitors; those types are included here as first-class schema citizens.
- Partner access to equivalent live data: [Bridge Interactive](https://bridgedataoutput.com) | [Zillow Tech Connect](https://www.zillowgroup.com/developers)
