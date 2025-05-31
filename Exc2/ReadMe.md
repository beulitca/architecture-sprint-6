1. minikube start
2. minikube addons enable metrics-server
3. Разбираемся, как называется metrics-server
kubectl get deployments --all-namespaces
NAMESPACE              NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
default                6era-deployment             1/1     1            1           70d
kube-system            coredns                     1/1     1            1           70d
kube-system            metrics-server              1/1     1            1           70d
kubernetes-dashboard   dashboard-metrics-scraper   1/1     1            1           70d
kubernetes-dashboard   kubernetes-dashboard        1/1     1            1           70d
4. Проверка метрикс-сервер по имени неймспейса
PS C:\Git\architecture-sprint-6\Exc2> kubectl get deployment metrics-server -n kube-system
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
metrics-server   1/1     1            1           70d
PS C:\Git\architecture-sprint-6\Exc2> 
5. Применяем hpa 
PS C:\Git\architecture-sprint-6\Exc2> kubectl apply -f hpa.yaml
horizontalpodautoscaler.autoscaling/scalable-pod-identifier-hpa configured
6. PS C:\Git\architecture-sprint-6\Exc2> kubectl get hpa
NAME                          REFERENCE                            TARGETS          MINPODS   MAXPODS   REPLICAS   AGE
scalable-pod-identifier-hpa   Deployment/scalable-pod-identifier   memory: 3%/10%   1         10        1          70d
7. PS C:\Git\architecture-sprint-6\Exc2> python -m locust                                          
[2025-05-30 17:58:03,549] DESKTOP-RJJPE5E/INFO/locust.main: Starting Locust 2.33.1
[2025-05-30 17:58:03,554] DESKTOP-RJJPE5E/INFO/locust.main: Starting web interface at http://localhost:8089, press enter to open your default browser
8. Получаем url сервиса для ввода в locust
minikube service service-6era --url
http://127.0.0.1:53831
9.  minikube dashboard
10. kubectl edit hpa scalable-pod-identifier-hpa 
во все поля Name вписать 6tera-deployment
11. Шаги по перезапуску миникуба с увеличенным количеством памяти....(приложение вместо загрузки процессора и памяти просто начинало отбивать входящие запросы при наличии ресурсов памяти и процессора, очень много времени ушло)