# test-resource-library-contracts-separate-jar (reproducer for [Mojarra #4254](https://github.com/javaserverfaces/mojarra/issues/4254))

## Steps to reproduce:

1. Clone the [test-resource-library-contracts-separate-jar](https://github.com/stiemannkj1/test-resource-library-contracts-separate-jar) project:

    ```
    git clone https://github.com/stiemannkj1/test-resource-library-contracts-separate-jar.git
    ```

2. Build and deploy the project to Liferay 7.0:

    ```
    (cd test-contract/ && mvn clean install) &&
        (cd test-jsf/ && mvn clean package -P liferay-snapshot,mojarra &&
            cp target/test-jsf*.war ~/Portals/liferay.com/7.0/deploy/test-jsf.war)
    ```

If the bugs still exists, the following exception will appear in the Liferay logs:

```
19:16:33,807 ERROR [fileinstall-/home/kylestiemann/Portals/liferay.com/7.0/osgi/war][com_liferay_portal_osgi_web_wab_extender:97] Catastrophic initialization failure! Shutting down test-jsf WAB due to: java.lang.StringIndexOutOfBoundsException: String index out of range: -1
java.lang.RuntimeException: java.lang.StringIndexOutOfBoundsException: String index out of range: -1
        at com.sun.faces.config.ConfigureListener.contextInitialized(ConfigureListener.java:292)
        at com.liferay.portal.osgi.web.wab.extender.internal.adapter.ServletContextListenerExceptionAdapter.contextInitialized(ServletContextListenerExceptionAdapter.java:51)
        at sun.reflect.GeneratedMethodAccessor723.invoke(Unknown Source)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.eclipse.equinox.http.servlet.internal.registration.ListenerRegistration$EventListenerInvocationHandler.invoke(ListenerRegistration.java:145)
        at com.sun.proxy.$Proxy490.contextInitialized(Unknown Source)
        at org.eclipse.equinox.http.servlet.internal.context.ContextController.doAddListenerRegistration(ContextController.java:357)
        at org.eclipse.equinox.http.servlet.internal.context.ContextController.addListenerRegistration(ContextController.java:310)
        at org.eclipse.equinox.http.servlet.internal.customizer.ContextListenerTrackerCustomizer.addingService(ContextListenerTrackerCustomizer.java:67)
        at org.eclipse.equinox.http.servlet.internal.customizer.ContextListenerTrackerCustomizer.addingService(ContextListenerTrackerCustomizer.java:1)
        at org.osgi.util.tracker.ServiceTracker$Tracked.customizerAdding(ServiceTracker.java:941)
        at org.osgi.util.tracker.ServiceTracker$Tracked.customizerAdding(ServiceTracker.java:1)
        at org.osgi.util.tracker.AbstractTracked.trackAdding(AbstractTracked.java:256)
        at org.osgi.util.tracker.AbstractTracked.track(AbstractTracked.java:229)
        at org.osgi.util.tracker.ServiceTracker$Tracked.serviceChanged(ServiceTracker.java:901)
        at org.eclipse.osgi.internal.serviceregistry.FilteredServiceListener.serviceChanged(FilteredServiceListener.java:109)
        at org.eclipse.osgi.internal.framework.BundleContextImpl.dispatchEvent(BundleContextImpl.java:917)
        at org.eclipse.osgi.framework.eventmgr.EventManager.dispatchEvent(EventManager.java:230)
        at org.eclipse.osgi.framework.eventmgr.ListenerQueue.dispatchEventSynchronous(ListenerQueue.java:148)
        at org.eclipse.osgi.internal.serviceregistry.ServiceRegistry.publishServiceEventPrivileged(ServiceRegistry.java:862)
        at org.eclipse.osgi.internal.serviceregistry.ServiceRegistry.publishServiceEvent(ServiceRegistry.java:801)
        at org.eclipse.osgi.internal.serviceregistry.ServiceRegistrationImpl.register(ServiceRegistrationImpl.java:127)
        at org.eclipse.osgi.internal.serviceregistry.ServiceRegistry.registerService(ServiceRegistry.java:225)
        at org.eclipse.osgi.internal.framework.BundleContextImpl.registerService(BundleContextImpl.java:464)
        at org.eclipse.osgi.internal.framework.BundleContextImpl.registerService(BundleContextImpl.java:482)
        at org.eclipse.osgi.internal.framework.BundleContextImpl.registerService(BundleContextImpl.java:1001)
        at com.liferay.portal.osgi.web.wab.extender.internal.WabBundleProcessor.initListeners(WabBundleProcessor.java:571)
        at com.liferay.portal.osgi.web.wab.extender.internal.WabBundleProcessor.init(WabBundleProcessor.java:203)
        at com.liferay.portal.osgi.web.wab.extender.internal.WebBundleDeployer._initWabBundle(WebBundleDeployer.java:186)
        at com.liferay.portal.osgi.web.wab.extender.internal.WebBundleDeployer.doStart(WebBundleDeployer.java:106)
        at com.liferay.portal.osgi.web.wab.extender.internal.WabFactory$WABExtension.start(WabFactory.java:158)
        at org.apache.felix.utils.extender.AbstractExtender.createExtension(AbstractExtender.java:259)
        at org.apache.felix.utils.extender.AbstractExtender.modifiedBundle(AbstractExtender.java:232)
        at org.osgi.util.tracker.BundleTracker$Tracked.customizerModified(BundleTracker.java:482)
        at org.osgi.util.tracker.BundleTracker$Tracked.customizerModified(BundleTracker.java:1)
        at org.osgi.util.tracker.AbstractTracked.track(AbstractTracked.java:232)
        at org.osgi.util.tracker.BundleTracker$Tracked.bundleChanged(BundleTracker.java:444)
        at org.eclipse.osgi.internal.framework.BundleContextImpl.dispatchEvent(BundleContextImpl.java:905)
        at org.eclipse.osgi.framework.eventmgr.EventManager.dispatchEvent(EventManager.java:230)
        at org.eclipse.osgi.framework.eventmgr.ListenerQueue.dispatchEventSynchronous(ListenerQueue.java:148)
        at org.eclipse.osgi.internal.framework.EquinoxEventPublisher.publishBundleEventPrivileged(EquinoxEventPublisher.java:165)
        at org.eclipse.osgi.internal.framework.EquinoxEventPublisher.publishBundleEvent(EquinoxEventPublisher.java:75)
        at org.eclipse.osgi.internal.framework.EquinoxEventPublisher.publishBundleEvent(EquinoxEventPublisher.java:67)
        at org.eclipse.osgi.internal.framework.EquinoxContainerAdaptor.publishModuleEvent(EquinoxContainerAdaptor.java:102)
        at org.eclipse.osgi.container.Module.publishEvent(Module.java:461)
        at org.eclipse.osgi.container.Module.start(Module.java:452)
        at org.eclipse.osgi.internal.framework.EquinoxBundle.start(EquinoxBundle.java:402)
        at org.apache.felix.fileinstall.internal.DirectoryWatcher.startBundle(DirectoryWatcher.java:1253)
        at org.apache.felix.fileinstall.internal.DirectoryWatcher.startBundles(DirectoryWatcher.java:1225)
        at org.apache.felix.fileinstall.internal.DirectoryWatcher.doProcess(DirectoryWatcher.java:512)
        at org.apache.felix.fileinstall.internal.DirectoryWatcher.process(DirectoryWatcher.java:361)
        at org.apache.felix.fileinstall.internal.DirectoryWatcher.run(DirectoryWatcher.java:312)
Caused by: java.lang.StringIndexOutOfBoundsException: String index out of range: -1
        at java.lang.String.substring(String.java:1967)
        at com.sun.faces.facelets.util.Classpath.searchFromURL(Classpath.java:235)
        at com.sun.faces.facelets.util.Classpath.searchFromURL(Classpath.java:242)
        at com.sun.faces.facelets.util.Classpath.search(Classpath.java:156)
        at com.sun.faces.config.WebConfiguration.discoverResourceLibraryContracts(WebConfiguration.java:489)
        at com.sun.faces.config.WebConfiguration.doPostBringupActions(WebConfiguration.java:461)
        at com.sun.faces.config.ConfigureListener.contextInitialized(ConfigureListener.java:269)
        ... 52 more
```

If the bug is fixed, the portlet will deploy without errors, and will render the following text when added to a page:

> Test: resourceLibraryContractsTest
> Status: SUCCESS
> Detail: Successfully rendered 2 different resources via Resource Library Contracts.

The background will also appear blue.
