# quote-service
Quotes microservice refactored from the SpringTrader application, as described in Part 3 of the *Refactoring a Monolith into a Cloud-Native Application* blog series.

This is a simple REST service that provides real-time-ish (15 minute delayed) market data. It  uses the public Yahoo Finance APIs. For more information on the underlying API, please refer to the documentation [here](https://developer.yahoo.com/yql).

The difference between this version of the service and the Part 2 version is this one registers itself with [Eureka](https://github.com/Netflix/eureka/wiki/Eureka-at-a-glance). 

The Eureka url is configured via the [manifest.yml](https://github.com/cf-platform-eng/quote-service/blob/part3live/manifest.yml) file.

Results are cached to improve response times, and due to the daily query limitations that Yahoo places on on its APIs. 

Additionally, the universe of supported securities is limited to those listed in the symbols.json [file](https://github.com/cf-platform-eng/quote-service/blob/part2/src/main/resources/symbols.json). This is because of the way SpringTrader currently tracks market data. The existing service API needs a way to get "all quotes" in one call: not practical with a universe of thousands of securities.

To get the source code, run the following from a clean directory:

```bash
git clone git@github.com:cf-platform-eng/quote-service.git
cd quote-service
git checkout part3live
```

Build the service using the maven conventions (from the root directory of the project):

```bash
mvn clean install
```

To run the service locally you can take advantage of the maven spring-boot plugin:

```bash
mvn spring-boot:run
```

Once it's running locally, try out some operations:

<http://localhost:8080/quotes/>

<http://localhost:8080/quotes/GOOG>

<http://localhost:8080/quotes/marketSummary>

<http://localhost:8080/quotes/topGainers>

<http://localhost:8080/quotes/topLosers>

<http://localhost:8080/quotes/symbols>


To deploy to cloud foundry, edit the manifest.yml file to give the app a unique name and to point it to your Eureka instance, and then perform a cf push.
