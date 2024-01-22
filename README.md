# Trip-Planner
A trip planning API that provides routing and searching services.
## Overview
Trip planner stores map data representing three kinds of items and answers three kinds of queries about them. The following two subsections describe the items and the queries in turn.
### Items
• A **position** has a latitude and a longitude, both numbers.   

• A **road segment** has two endpoints, both positions.

• A **point-of-interest** (POI) has a position, a category (a string), and a name (a string). The name of a point-of-interest is unique across all points-of- interest, but a category may be shared by multiple points-of-interest. Each position can feature zero, one, or more POIs.
### Assumptions:
1. All roads are two-way roads.
2. The length of a road segment is the standard Euclidean distance between its endpoints.
3. Points-of-interest can only be found at a road segment endpoint.
### Queries
Trip planner supports three forms of queries:

**locate-all** Takes a point-of-interest category; returns the positions of all points-of-interest in the given category. The positions can be returned in any order, but the result does not include duplicates.

**plan-route** Takes a starting position (latitude and longitude) and the name of a point-of-interest; returns a shortest path from the starting position to the named point-of-interest. It's assumed the starting position is at a road segment endpoint. Returns the empty list if the destination does not exist or is unreachable.

**find-nearby** Takes a starting position (latitude and longitude), a point-of-interest category, and a limit n; returns the (up to) n points-of-interest in the given category nearest the starting position. It's assumed the starting position is at a road segment endpoint.

### API Specification
The trip planner API is specified as a DSSL2 interface named TRIP_PLANNER, which is implemented as a class named TripPlanner.
### Basic Types
The basic vocabulary types include latitude and longitude (represented as numbers) and POI categories and names (represented as strings).
### Raw Item Types
In the external API that the TripPlanner class must support, items are represented in “raw” form, i.e., as vectors (not structs) of basic types. Specifically:

A **raw position** is represented as a 2-element vector containing a latitude and a longitude.

A **raw road segment** is represented as a 4-element vector containing the latitude and longitude of one end position followed by the latitude and longitude of the other.

A **raw point-of-interest** is represented as a 4-element vector containing the latitude and longitude of its position, then its category, and then its name. These types are intended for communication between the TripPlanner class and the client.
### Input Types
The TripPlanner constructor takes two arguments:
A vector of raw road segments.
A vector or raw points-of-interest.
### Output Types
The results for each of the three kinds of queries are expressed in terms of the raw item types. Specifically:

**locate-all** returns its result as a linked list of raw positions. The elements of the list can be returned in any order, and each position only appears once in the result.

**plan-route** also returns a linked list of raw positions. The positions are in the order in which they appear in the path from the source to the destination. For non-existent or unreachable destinations, plan-route returns the empty list.

**find-nearby** returns a linked list of raw points-of-interest.
