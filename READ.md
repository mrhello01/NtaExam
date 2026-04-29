#Program1[w/o sklearn]:
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
df = pd.read_csv("housing.csv")
numerical_features = df.select_dtypes(include=[np.number]).columns
plt.figure(figsize=(15, 10))
for i, feature in enumerate(numerical_features):
    plt.subplot(3, 3, i + 1)
    sns.histplot(df[feature], kde=True, bins=30)
    plt.title(f'Distribution of {feature}')
plt.tight_layout()
plt.show()
plt.figure(figsize=(15, 10))
for i, feature in enumerate(numerical_features):
    plt.subplot(3, 3, i + 1)
    sns.boxplot(x=df[feature])
    plt.title(f'Box Plot of {feature}')
plt.tight_layout()
plt.show()
print("Outliers Detection:")
for feature in numerical_features:
    Q1 = df[feature].quantile(0.25)
    Q3 = df[feature].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    count = ((df[feature] < lower) | (df[feature] > upper)).sum()
    print(f"{feature}: {count} outliers")

#Program2[w/o sklearn]
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
data = pd.read_csv("housing.csv")
correlation = data.corr(numeric_only=True)
plt.figure(figsize=(10, 8))
sns.heatmap(correlation, annot=True, cmap='coolwarm', fmt='.2f')
plt.title("Correlation Matrix")
plt.show()
sns.pairplot(data.sample(500), diag_kind='kde')
plt.suptitle("Pair Plot Of California Housing Features", y=1.02)
plt.show()

#Program3[w/o sklearn]
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
data = pd.read_csv("IRIS Flower.csv")
x = data.drop(columns=['species'])
y = data['species']
x_pca = PCA(n_components=2).fit_transform(x)
labels, uniques = pd.factorize(y)
colors = ['r', 'g', 'b']
plt.figure(figsize=(8, 6))
for i in range(len(uniques)):
    plt.scatter(
        x_pca[labels == i, 0],
        x_pca[labels == i, 1],
        color=colors[i],
        label=uniques[i]
    )
plt.title("PCA on Iris Dataset")
plt.xlabel("PC1")
plt.ylabel("PC2")
plt.legend()
plt.grid()
plt.show()

#Program4
import pandas as pd
import numpy as np
def find_s_algorithm(data):
    h = None  
    for _, row in data.iterrows():
        if str(row.iloc[-1]).lower() == "yes":             
            if h is None:
                h = row.iloc[:-1].values                   
            else:
                for i in range(len(h)):
                    if h[i] != row.iloc[i]:      
                        h[i] = "?"    
    return h
df = pd.read_csv("training_data.csv")
print("Training Data:\n", df)
print("\nFinal Hypothesis:", find_s_algorithm(df))

#Program5
import numpy as np
import matplotlib.pyplot as plt
np.random.seed(42)
data = np.random.rand(100)
train = data[:50]
labels = ["Class1" if x <= 0.5 else "Class2" for x in train]
test = data[50:]
def knn(x, k):
    d = sorted([(abs(x-train[i]), labels[i]) for i in range(50)])
    l = [i[1] for i in d[:k]]
    return "Class1" if l.count("Class1") > l.count("Class2") else "Class2"
k_values = [1,2,3,4,5,20,30]
preds1 = [knn(x,1) for x in test]
c1 = [test[i] for i in range(50) if preds1[i]=="Class1"]
c2 = [test[i] for i in range(50) if preds1[i]=="Class2"]
plt.scatter(train,[0]*50,c=["blue" if l=="Class1" else "red" for l in labels])
plt.scatter(c1,[1]*len(c1),marker='x')
plt.scatter(c2,[1]*len(c2),marker='x')
plt.title("KNN (k=1)")
plt.xlabel("Data Points")
plt.ylabel("Class Level")
plt.grid()
plt.show()
for k in k_values:
    print("\nResults for k =", k)    
    for i in range(len(test)):
        result = knn(test[i], k)
        print("x", i+51, "(", round(test[i],4), ") ->", result)

#Program6
import numpy as np
import matplotlib.pyplot as plt
def gaussian_kernel(x, xi, tau):
    return np.exp(-np.sum((x - xi)**2) / (2 * tau**2))
def lwr(x, X, y, tau):
    m = X.shape[0]
    weights = np.array([gaussian_kernel(x, X[i], tau) for i in range(m)])
    W = np.diag(weights)
    theta = np.linalg.inv(X.T @ W @ X) @ X.T @ W @ y
    return x @ theta
np.random.seed(42)
X = np.linspace(0, 2*np.pi, 100)
y = np.sin(X) + 0.1*np.random.randn(100)
X_bias = np.c_[np.ones(X.shape), X]
x_test = np.linspace(0, 2*np.pi, 200)
x_test_bias = np.c_[np.ones(x_test.shape), x_test]
taus = [0.3, 0.8, 2.0]
plt.scatter(X, y, color='red', label='Data')
for tau in taus:
    y_pred = np.array([lwr(xi, X_bias, y, tau) for xi in x_test_bias])
    plt.plot(x_test, y_pred, label=f"tau = {tau}")
plt.xlabel("X")
plt.ylabel("y")
plt.title("Locally Weighted Regression")
plt.legend()
plt.show()

#Program7[w/o sklearn]
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import PolynomialFeatures, StandardScaler
from sklearn.pipeline import make_pipeline
from sklearn.metrics import mean_squared_error, r2_score
data = pd.read_csv("BostonHousing.csv")
x= data[["rm"]]
y = data["medv"]
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2)
model = LinearRegression()
model.fit(x_train, y_train)
y_pred = model.predict(x_test)
plt.scatter(x_test, y_test)
plt.plot(x_test, y_pred)
plt.title("Linear Regression - Boston Housing")
plt.show()
print("Linear Regression")
print("MSE:", mean_squared_error(y_test, y_pred))
print("R2:", r2_score(y_test, y_pred))
data = pd.read_csv("auto-mpg.csv")
data = data.dropna()
x = data[["displacement"]]
y = data["mpg"]
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2)
model = make_pipeline(PolynomialFeatures(2), StandardScaler(), LinearRegression())
model.fit(x_train, y_train)
y_pred = model.predict(x_test)
plt.scatter(x_test, y_test)
plt.scatter(x_test, y_pred)
plt.title("Polynomial Regression - Auto MPG")
plt.show()
print("Polynomial Regression")
print("MSE:", mean_squared_error(y_test, y_pred))
print("R2:", r2_score(y_test, y_pred))

#Program8[w/o sklearn]
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
from sklearn import tree
data = pd.read_csv("Breast Cancer Dataset.csv")   
data = data.drop(columns=['id'])
x = data.drop(columns=['diagnosis'])
y = data['diagnosis']
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2, random_state=42)
clf = DecisionTreeClassifier(random_state=42)
clf.fit(x_train, y_train)
y_pred = clf.predict(x_test)
print("Model Accuracy:", accuracy_score(y_test, y_pred) * 100)
new_sample = x_test.iloc[[0]]
prediction = clf.predict(new_sample)
print("Predicted Class:", "Benign" if prediction[0] == 1 else "Malignant")
plt.figure(figsize=(12, 8))
tree.plot_tree(clf, filled=True, feature_names=x.columns, class_names=["Malignant", "Benign"])
plt.show()

#Program9[w/o sklearn]
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
faces = np.load("olivetti_faces.npy")
targets = np.load("olivetti_faces_target.npy")
print("Faces shape:", faces.shape)
print("Targets shape:", targets.shape)
X = faces.reshape((faces.shape[0], -1))
y = targets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
model = GaussianNB()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
print("\nAccuracy:", accuracy_score(y_test, y_pred) * 100)
print("\nClassification Report:")
print(classification_report(y_test, y_pred, zero_division=1))
print("\nConfusion Matrix:")
print(confusion_matrix(y_test, y_pred))
cv = cross_val_score(model, X, y, cv=5)
print("\nCross-validation accuracy:", cv.mean() * 100)
fig, axes = plt.subplots(3, 5, figsize=(12, 8))
for ax, img, t, p in zip(axes.ravel(), X_test, y_test, y_pred):
    ax.imshow(img.reshape(64, 64), cmap='gray')
    ax.set_title(f"T:{t} P:{p}")
    ax.axis('off')
plt.show()

#Program10
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
    ax.axis('off')

plt.show()
