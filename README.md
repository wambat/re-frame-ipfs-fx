# IPFS Re-Frame Effectful Event Handlers

[![Build Status](https://travis-ci.org/district0x/re-frame-ipfs-fx.svg?branch=master)](https://travis-ci.org/district0x/re-frame-ipfs-fx)

ReFrame handlers for [IPFS library](https://github.com/district0x/cljs-ipfs-native) 


## Installation
```clojure
;; Add to dependencies
[district0x/re-frame-ipfs-fx "1.0.0"]
```
```clojure
(ns my.app.handlers
  (:require 
  [district0x.re-frame-ipfs-fx.ipfs-fx :as core]))
```

## Usage
The library relies on HTTP API signatures, so follow this [docs](https://github.com/ipfs/js-ipfs-api#api), Also, return values and responses in callbacks are automatically kebab-cased and keywordized. You can provide IPFS instance as an :inst map entry, in case you'd need more than one connection:


### Example call
```clojure
  (reg-event-fx
  ::init-ipfs
  interceptors
  (fn [_ [url]]
    {:ipfs/init nil}))                                                                     
   
  (reg-event-fx
  ::list-files
  interceptors
  (fn [_ [url]]
    {:ipfs/call {:func "ls"
                :args [url]
                :on-success ::on-list-files-success
                :on-error ::error}}))  
  (reg-event-fx
  ::on-list-files-success
  interceptors
  (fn [{:keys [:db]} [data]]
    {:db (assoc db :files data)}))
    
;;And then
  (dispatch [::init-ipfs])
  (dispatch [::list-files "/ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/"])
```
