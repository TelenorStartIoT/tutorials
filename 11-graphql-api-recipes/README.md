# GraphQL Query Recipes

This document contains common GraphQL query recipes for you to use or take example of. You should read the "Managed IoT Cloud - GraphQL API" first so you know how to run the following queries.

**NB! This code example is for prototyping only. Not for deployment in production.**

## 1. Query all rules available to the currently authorized user. Sort by rule name.

```js
MIC.graphql({
  query: `
    query ($sort: String) {
      allRules(searchOptions: { sort: $sort }) {
        name
        description
        id
        enabled
      }
    }
  `,
  variables: {
    sort: 'name'
  }
})
```

## 2. Query all resources created for (or by) the Thing Type "490".

```js
MIC.graphql({
  query: `
    query thingTypeResources($thingTypeId: String!) {
      thingTypeResources(thingTypeId: $thingTypeId) {
        resources {
          id
          name
          type
          metadata {
            label
            unit
            isSettable
            isVirtual
            custom {
              label
              value
            }
          }
        }
      }
    }
  `,
  variables: {
    thingTypeId: '490'
  }
})
```

## 3. Query for the first page of all online Things for the Thing Type ID "490". Use "Oslo/Europe" as the time-zone and sort by Thing label name.

```js
MIC.graphql({
  query: `
    query allThings(
      $searchOptions: ListThingsSearchOptions,
      $timezone: String
    ) {
      allThings(searchOptions: $searchOptions) {
        pageInfo {
          page
          pages
          size
          itemCount
          totalCount
        }
        things {
          thingName
          label
          domain
          domainConnection {
            name
          }
          thingTypeConnection {
            label
          }
          thingType
          lastHeardFrom(
            momentFunction: {
              name: fromNow,
              arguments: "true",
              timezone: $timezone
            }
          )
          createdAt(
            momentFunction: {
              name: fromNow,
              arguments: "true",
              timezone: $timezone
            }
          )
          state
          description
          hasNetworkedThings
          parentThingName
        }
      }
    }
  `,
  variables: {
    searchOptions: {
      filter: {
        thingTypes: ['490'],
        thingStatus: -1
      },
      paging: {
        page: 1
      },
      sorting: {
        sort: 'label.lowercase'
      }
    },
    timezone: 'Europe/Oslo'
  }
})
```

## 4. Query for all information of a Thing with the Name "00001341".

```js
MIC.graphql({
  query: `
    query thing($thingName: String!) {
      thing(thingName: $thingName) {
        shadow
        domainTopic
        domain
        label
        thingName
        createdAt
        thingType
        parentThingName
        hasNetworkedThings
        description
        thingTypeConnection {
          resourcesConnection {
            resources {
              id
              name
              type
              subthingType
              metadata {
                label
                unit
                isSettable
                isVirtual
                custom {
                  label
                  value
                }
              }
            }
          }
        }
        networkedThingsConnection {
          pageInfo {
            page
            pages
            size
            itemCount
            totalCount
          }
          things {
            label
            thingName
            thingType
            hasNetworkedThings
          }
        }
      }
    }
  `,
  variables: {
    thingName: '00001341'
  }
})
```

## 5. Query observations (using Elasticsearch) for a Thing with Name "00001341" and Type "490". Restrict the observation results to 100 hit per page and a resource called "pm25". Specify a "from" and "to" date. Sort by timestamp in descending order.

```js
MIC.graphql({
  query: `
    query (
      $thingName: String!,
      $thingType: String!,
      $resourceStatePath: String!,
      $from: String!,
      $to: String!,
      $timestampFormat: String,
      $timezone: String
    ) {
      thingObservations(
        searchOptions: {
          filter: {
            thingName: $thingName,
            period: {
              from: $from,
              to: $to
            },
            resources: [$resourceStatePath],
            thingTypes: [$thingType]
          },
          paging: {
            size: 100
          },
          sorting: {
            sort: "-timestamp"
          }
        }
      ) {
        pageInfo {
          totalCount
        }
        observations {
          timestamp(
            momentFunction: {
              name: format,
              arguments: [$timestampFormat],
              timezone: $timezone
            }
          )
          state
        }
      }
    }
  `,
  variables: {
    thingType: '490',
    thingName: '00001341',
    resourceStatePath: 'pm25',
    from: '2019-03-09T23:00:00.000Z',
    to: '2019-03-17T20:58:53.922Z',
    timezone: 'Europe/Oslo'
  }
})
```
