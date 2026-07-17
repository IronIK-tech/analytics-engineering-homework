
# MongoDB Queries & API Integration

Practice working with MongoDB (Atlas, sample_mflix dataset) using pymongo: filtering, regex search, aggregation pipelines, and loading external API data into a collection.

# Topics Covered:

- Connecting to MongoDB Atlas with pymongo
- Filtering with $gt, $lt, $or
- Regex search ($regex, $options)
- distinct() for unique field values
- Aggregation pipeline: $group, $unwind, $sort, $limit
- Computed fields with $addFields and $cond (nested conditional logic)
- Loading data from a public REST API (NBU exchange rates) and inserting it into a collection with insert_many
- Clearing a collection before reload (delete_many({}))
