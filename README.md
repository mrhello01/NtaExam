# Program1
import pandas as pd
from sklearn.datasets import fetch_california_housing
df = fetch_california_housing(as_frame=True).frame
outliers = {}
for col in df.select_dtypes(include='number'):
    Q1, Q3 = df[col].quantile([0.25, 0.75])
    IQR = Q3 - Q1
    outliers[col] = ((df[col] < Q1 - 1.5*IQR) | (df[col] > Q3 + 1.5*IQR)).sum()
print("Outliers per feature:\n", outlie
print("\nSummary:\n", df.describe())

import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_california_housing
df = fetch_california_housing(as_frame=True).frame
cols = df.columns
df.hist(figsize=(12, 8), bins=30)
plt.show()
df.plot(kind='box', subplots=True, layout=(3, 3), figsize=(12, 8))
plt.tight_layout()
plt.show()

# Program2
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_california_housing
df = fetch_california_housing(as_frame=True).frame
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')
plt.show()
sns.pairplot(df)
plt.show()

# Program3
from sklearn.datasets import load_iris
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
d = load_iris()
X = PCA(2).fit_transform(d.data)
print("Explained variance ratio:", PCA(2).fit(d.data).explained_variance_ratio_)
for i, c in enumerate(['r','g','b']):
    plt.scatter(X[d.target==i,0], X[d.target==i,1], c=c, label=d.target_names[i])
plt.legend() 
plt.grid()
plt.show()

# Program4
import pandas as pd
df = pd.read_csv('training_data.csv')
print("Training data:")
print(df)
h = ['?']*(df.shape[1]-1)
for _, r in df.iterrows():
    if r.iloc[-1] == 'Yes':
        for i in range(len(h)):
            if h[i] == '?' or h[i] == r.iloc[i]:
                h[i] = r.iloc[i]
            else:
                h[i] = '?'
print("\nThe final hypothesis is:", h)

# Program5
import numpy as np, matplotlib.pyplot as plt
from collections import Counter
d=np.random.rand(100);
tr,te=d[:50],d[50:]
lab=["Class1" if x<=0.5 else "Class2" for x in tr]
def knn(x,k): 
    return Counter([l for _,l in sorted((abs(x-tr[i]),lab[i]) 
                                        for i in range(len(tr)))[:k]]).most_common(1)[0][0]
pred=[knn(x,3) for x in te]
plt.scatter(tr,[0]*50,c=['b' if l=="Class1" else 'r' for l in lab],marker='o',label='Train (o)')
plt.scatter(te,[1]*50,c=['b' if p=="Class1" else 'r' for p in pred],marker='x',label='Test (x)')
plt.legend(); plt.show()

# Program6
import numpy as np
import matplotlib.pyplot as plt
X=np.linspace(0,2*np.pi,100)
y=np.sin(X)+0.1*np.random.randn(100)
Xb=np.c_[np.ones(X.shape),X]
tau=0.5
def lwr(x):
    w=np.exp(-np.sum((Xb-x)**2,axis=1)/(2*tau**2))
    W=np.diag(w)
    return x @ (np.linalg.inv(Xb.T@W@Xb)@Xb.T@W@y)
xt=np.linspace(0,2*np.pi,200)
xtb=np.c_[np.ones(xt.shape),xt]
yp=[lwr(x) for x in xtb]
plt.scatter(X,y,color='red',label='Training Data')
plt.plot(xt,yp,color='blue',label='LWR Fit')
plt.legend()
plt.show()

# Program7
import pandas as pd, matplotlib.pyplot as plt
from sklearn.datasets import fetch_california_housing
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import PolynomialFeatures
from sklearn.metrics import mean_squared_error
X=fetch_california_housing(as_frame=True).frame[["AveRooms"]]
y=fetch_california_housing(as_frame=True).target
Xtr,Xte,ytr,yte=train_test_split(X,y,test_size=0.2)
lr=LinearRegression().fit(Xtr,ytr)
yp=lr.predict(Xte)
plt.scatter(Xte,yte,color='red',label="Actual")
plt.plot(Xte,yp,label="Linear")
plt.legend()
plt.show()
print("LR MSE:",mean_squared_error(yte,yp))
poly=PolynomialFeatures(2)
Xtr_p=poly.fit_transform(Xtr)
Xte_p=poly.transform(Xte)
pr=LinearRegression().fit(Xtr_p,ytr)
yp2=pr.predict(Xte_p)
plt.scatter(Xte,yte,label="Actual")
plt.scatter(Xte,yp2,label="Poly")
plt.legend()
plt.show()
print("Poly MSE:",mean_squared_error(yte,yp2))

# Program8
import matplotlib.pyplot as plt
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.metrics import accuracy_score
X,y = load_breast_cancer(return_X_y=True)
Xtr,Xte,ytr,yte = train_test_split(X,y,test_size=0.2)
clf = DecisionTreeClassifier().fit(Xtr,ytr)
print("Accuracy:", accuracy_score(yte, clf.predict(Xte)))
plot_tree(clf, filled=True)
plt.show()

# Progrma9
import numpy as np
from sklearn.datasets import fetch_olivetti_faces
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
import matplotlib.pyplot as plt
data = fetch_olivetti_faces(shuffle=True, random_state=42)
X = data.data
y = data.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
gnb = GaussianNB()
gnb.fit(X_train, y_train)
y_pred = gnb.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
print(f'Accuracy: {accuracy * 100:.2f}%')
print("\nClassification Report:")
print(classification_report(y_test, y_pred, zero_division=1))
print("\nConfusion Matrix:")
print(confusion_matrix(y_test, y_pred))
cross_val_accuracy = cross_val_score(gnb, X, y, cv=5, scoring='accuracy')
print(f'\nCross-validation accuracy: {cross_val_accuracy.mean() * 100:.2f}%')
fig, axes = plt.subplots(3, 5, figsize=(12, 8))
for ax, image, label, prediction in zip(axes.ravel(), X_test, y_test, y_pred):
    ax.imshow(image.reshape(64, 64), cmap=plt.cm.gray)
    ax.set_title(f"True: {label}, Pred: {prediction}")
    ax.axis('off')
plt.show()

# Program10
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import load_breast_cancer
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.metrics import confusion_matrix, classification_report
data = load_breast_cancer()
X = data.data
y = data.target
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
kmeans = KMeans(n_clusters=2, random_state=42)
y_kmeans = kmeans.fit_predict(X_scaled)
print("Confusion Matrix:")
print(confusion_matrix(y, y_kmeans))
print("\nClassification Report:")
print(classification_report(y, y_kmeans))
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)
df = pd.DataFrame(X_pca, columns=['PC1', 'PC2'])
df['Cluster'] = y_kmeans
df['True Label'] = y
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x='PC1', y='PC2', hue='Cluster', palette='Set1', s=100, edgecolor='black', alpha=0.7)
plt.title('K-Means Clustering of Breast Cancer Dataset')
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.legend(title="Cluster")
plt.show()
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x='PC1', y='PC2', hue='True Label', palette='coolwarm', s=100, edgecolor='black', alpha=0.7)
plt.title('True Labels of Breast Cancer Dataset')
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.legend(title="True Label")
plt.show()
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x='PC1', y='PC2', hue='Cluster', palette='Set1', s=100, edgecolor='black', alpha=0.7)
centers = pca.transform(kmeans.cluster_centers_)
plt.scatter(centers[:, 0], centers[:, 1], s=200, c='red', marker='X', label='Centroids')
plt.title('K-Means Clustering with Centroids')
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.legend(title="Cluster")
plt.show()
