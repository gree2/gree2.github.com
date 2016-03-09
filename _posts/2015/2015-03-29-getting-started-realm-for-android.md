---
layout: post
title: "getting started realm for android"
description: ""
category: [android]
tags: [lib, database, realm]
---
{% include JB/setup %}

### prerequisites

* do not support java outside of android
* android studio >= 0.8.6
* android sdk
* jdk version >=7
* support all android versions since api level 9 (android 2.3 gingerbread & above)

### installation

1. maven

    1. use jcenter as a dependency repo
    1. add `compile 'io.realm:realm-android:0.80.0'` to the dependencies of your project
    1. sync you project

1. jar

    1. [download](http://static.realm.io/downloads/java/realm-java-0.80.0.zip) package and unzip
    1. copy the `realm-version.jar` into app/libs
    1. sync you project

### proguard

add the configuration below to your proguard configuration file

    -keepnames public class * extends io.realm.RealmObject
    -keep class io.realm.** { *; }
    -dontwarn javax.**
    -dontwarn io.realm.**

### models

1. BusLine

        public class BusLine extends RealmObject {

            @PrimaryKey
            private long id;
            private int lineIndex = 0;
            private String lineName = "";
            private String lineCode = "";

            private RealmList<BusStop> busStops;

            public long getId() {
                return id;
            }

            public void setId(long id) {
                this.id = id;
            }

            ...

            public void setBusStops(RealmList<BusStop> busStops) {
                this.busStops = busStops;
            }
        }

1. BusStop

        public class BusStop extends RealmObject {

            @PrimaryKey
            private long id = 0;
            private long busLineId = 0;
            private int stopIndex = 0;
            private String stopName = "";
            private String stopDesc = "";
            private String class07 = "";
            private String class09 = "";
            private String class13 = "";
            private String class17 = "";
            private String distance = "";
            private String distanceEta = "";
            private String stopImage = "";

            public long getId() {
                return id;
            }

            public void setId(long id) {
                this.id = id;
            }

            ...

        }

### writes

    Realm realm = Realm.getInstance(context);
    realm.beginTransaction();
    Config config = realm.createObject(Config.class);
    config.setKey("sync");
    config.setValue("true");
    realm.commitTransaction();

### queries

    final Realm realm = Realm.getInstance(this);
    busLines = realm.where(BusLine.class).findAll();
    busLines.sort("id");

### notifications

    // after sync refresh ui
    realm.addChangeListener(new RealmChangeListener() {
        @Override
        public void onChange() {
            busLines = realm.where(BusLine.class).findAll();
            busLines.sort("id");
            for (Object object : busLines.toArray()) {
                navDrawerItems.add(new NavDrawerItem(((BusLine)object).getId() + " " + ((BusLine)object).getLineName()));
            }
            adapter.notifyDataSetChanged();
        }
    });

### adapter

    public class BusStopAdapter extends RealmBaseAdapter<BusStop> implements ListAdapter {

        private static class ViewHolder {
            TextView stopName;
        }

        public BusStopAdapter(Context context, RealmResults<BusStop> realmResults, boolean automaticUpdate) {
            super(context, realmResults, automaticUpdate);
        }

        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            ViewHolder viewHolder;
            if (convertView == null) {
                LayoutInflater inflater = (LayoutInflater) this.context
                        .getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                convertView = inflater.inflate(R.layout.fragment_bus_stop_item, parent, false);
                viewHolder = new ViewHolder();
                viewHolder.stopName = (TextView) convertView.findViewById(R.id.stop_name);
                convertView.setTag(viewHolder);
            } else {
                viewHolder = (ViewHolder) convertView.getTag();
            }

            BusStop busStop = realmResults.get(position);
            viewHolder.stopName.setText(busStop.getStopIndex() + " " + busStop.getStopName());
            return convertView;
        }

        public RealmResults<BusStop> getRealmResults() {
            return realmResults;
        }

    }

### references

1. [realm java doc](http://realm.io/docs/java/0.80.0)

1. [EasternPioneerBus](https://github.com/gree2/EasternPioneerBus/commit/c05ef7bd0f89a3b8a00dc8adab4dba353165364b)
