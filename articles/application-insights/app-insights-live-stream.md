---
title: Live Metrics Stream in Azure Application Insights | Microsoft Docs
description: Monitor the performance of your web app in real time, to observe the effects of a release or other change.
services: application-insights
documentationcenter: ''
author: alancameronwills
manager: carmonm

ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
<<<<<<< HEAD
ms.date: 02/08/2017
=======
ms.date: 02/22/2017
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a
ms.author: awills

---

# Live Metrics Stream: instant metrics for close monitoring
Live Metrics Stream shows you your [Application Insights](app-insights-overview.md) metrics right at this very moment, with a near-real-time latency of one second. This immediate monitoring is very useful when you’re releasing a new build and want to make sure that everything is working as expected, or investigating an incident in real time.

<<<<<<< HEAD
![In the Overview blade, click Live Stream](./media/app-insights-live-stream/live-stream.png)

=======
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a
Unlike [Metrics Explorer](app-insights-metrics-explorer.md), Live Metrics Stream displays a fixed set of metrics. The data persists only for as long as it's on the chart, and is then discarded.

Live Metrics Stream data is free: it doesn't add to your bill.

<<<<<<< HEAD
=======
![Live Metrics Stream video](./media/app-insights-live-stream/youtube.png) [Live Metrics Stream video](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

![In the Overview blade, click Live Stream](./media/app-insights-live-stream/live-stream.png)



>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a
## Live failures

If any failures or exceptions are logged, Live Stream picks out a sample of them. Click **Pause** to hold a specific sample, and select an event to show its details.

![Sampled live failures](./media/app-insights-live-stream/live-stream-failures.png)


Live Metrics Stream is available with the latest version of [Application Insights SDK for web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).



## Troubleshooting

No data? Live Metrics Stream uses a different port than other Application Insights telemetry. Make sure [those ports](app-insights-ip-addresses.md) are open in your firewall.



## Next steps
* [Monitoring usage with Application Insights](app-insights-overview-usage.md)
* [Using Diagnostic Search](app-insights-diagnostic-search.md)

