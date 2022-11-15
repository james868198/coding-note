# SLI SLO SLA

- video: The Art of SLOs
- google tutorial: [link](https://cloud.google.com/blog/products/management-tools/practical-guide-to-setting-slos)

## what is SLI SLO SLA

---

define SLIs: metric over time
ex: 95% 的 request latency last 5 mins < 300ms

define SLOs: upon of SLI

bind target for a collection of SLIs
ex: 95% 的 sli success > 99.9%

define SLAs: agreement/punishment when SLOs fail

SLIs drive SLOs which inform SLAs

---

how to choose SLI?

good SLI should rise when customers are happier

## how to choose SLIs

---

most common: latency, error rate, throughput, availability and response time

- request/response
  - availability
  - latency
  - quality
- data processing
  - coverage
  - correctness
  - freshness
  - throughput
- storage
  - throughput
  - latency

## Steps

---

1.  List out critical user journeys and order them by business impact

    排列 user story

    PE Manager approve change request and change access
    PE create/import change request
    PE search data

2.  Determine which metrics to use as SLIs to most accurately track the user experience

    most important user story: PE Manager approve change request and change access

    - Availability SLI

    how many PE Manager approve change request. how many of those requests succeeded.

    The proportion of HTTP GET requests for /checkout_service/response_counts that do not have 5XX status (3XX and 4XX excluded) measured at the Istio service mesh.

    - correctness SLI

    The proportion of valid data that produced correct output.

        1.alarm amount measurement with multiple type
        2.invalid data error

    - latency SLI

    200 milliseconds and 1 second

3.  Determine SLO target goals and SLO measurement period

    multiple grades of SLOs for some types of SLIs.

4.  Create SLI, SLO, and error budget consoles

5.  Create SLO alerts
