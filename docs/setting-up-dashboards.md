# Setting up a dashboard

## Create new project

create a new clojure(script) project using lein. We recommned using the chestnut lein template.

```

lein new chestnut <your-dashboard-name> -- --vanilla --http-kit

```

## Set up dashboard-clj as a dependecy

Add dashbaord-clj in your project.clj dependencies

```

:dependecies [[dashboard-clj "0.1.0-SNAPSHOT"]]

```

## Setup datasources

### Configure jobs

A job requires a data source and a schedule. Data source is the way to collect data periodically as per the schedule and publish it to the dashboard. You could use the following configuration to setup a job,

```

(def jobs [{
:data-source {
                :name banglore-weather
                :fn :dashboard-clj.plugins.weather.core/fetch
                :args [{:city "Bangalore"}]
             }
:schedule {
                :type :repeat
                :seconds 10
                :minutes 0
                :hours 0
                :start-in 0
}}])

```

### Start jobs

```

(dashboard-clj.job/start jobs)

```

## setup dashboards

go to the main cljs namespace in your application. If your project name is `project-health-dashboard`, this might be `project-health-dashboard.core`

### A simple dashboard with two widgets, which uses a grid layout

```

(ns project-health-dashboard.core
    (:require [dashboard-clj.core :as dash]))

(def layout (dash/grid-layout
                [dash/widgets.static-text-widget {:name :static-widget-one :text "Hello" :post {:x 0 :y 0 :h 1 :w 2 }}]
                [dash/widgets.static-text-widget {:name :static-widget-two :text "World!!" :pos {:x 0 :y 1 :h 1 :w 2}}]))
    
```

