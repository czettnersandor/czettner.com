---
layout: post
title: Analytics Page Speed Cheat
created: 1383127647
comments: true
---
<code>
    timing = {
        navigationStart: window.performance.timing.navigationStart+450,
        unloadEventStart: window.performance.timing.unloadEventStart,
        unloadEventEnd: window.performance.timing.unloadEventEnd,
        redirectStart: window.performance.timing.redirectStart,
        redirectEnd: window.performance.timing.redirectEnd,
        fetchStart: window.performance.timing.fetchStart,
        domainLookupStart: window.performance.timing.domainLookupStart,
        domainLookupEnd: window.performance.timing.domainLookupEnd,
        connectionStart: window.performance.timing.connectionStart,
        secureConnectionStart: window.performance.timing.secureConnectionStart,
        requestStart: window.performance.timing.requestStart,
        responseStart: window.performance.timing.responseStart,
        responseEnd: window.performance.timing.responseEnd,
        domLoading: window.performance.timing.domLoading,
        domInteractive: window.performance.timing.domInteractive,
        domContentLoadedEventStart: window.performance.timing.domContentLoadedEventStart,
        domContentLoadedEventEnd: window.performance.timing.domContentLoadedEventEnd,
        domComplete: window.performance.timing.domComplete,
        loadEventStart: window.performance.timing.loadEventStart,
        loadEventEnd: window.performance.timing.loadEventEnd
    }

    window.performance = { timing: timing, navigation: window.performance.navigation }
</code>
