# Movie-Recommendation

{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "from scipy import sparse\n",
    "from sklearn.metrics.pairwise import cosine_similarity\n",
    "ratings=pd.read_csv(\"toy_dataset.csv\",index_col=0)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>action1</th>\n",
       "      <th>action2</th>\n",
       "      <th>action3</th>\n",
       "      <th>romantic1</th>\n",
       "      <th>romantic2</th>\n",
       "      <th>romantic3</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>user 1</th>\n",
       "      <td>4.0</td>\n",
       "      <td>5.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>1.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>user 2</th>\n",
       "      <td>5.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>user 3</th>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>4.0</td>\n",
       "      <td>5.0</td>\n",
       "      <td>4.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>user 4</th>\n",
       "      <td>0.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>4.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>3.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>user 5</th>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>4.0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "        action1  action2  action3  romantic1  romantic2  romantic3\n",
       "user 1      4.0      5.0      3.0        0.0        2.0        1.0\n",
       "user 2      5.0      3.0      3.0        2.0        2.0        0.0\n",
       "user 3      1.0      0.0      0.0        4.0        5.0        4.0\n",
       "user 4      0.0      2.0      1.0        4.0        0.0        3.0\n",
       "user 5      1.0      0.0      2.0        3.0        3.0        4.0"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "ratings.fillna(0, inplace=True)\n",
    "ratings"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "           user 1  user 2  user 3    user 4    user 5\n",
      "action1      0.36    0.56   -0.24 -0.440000 -0.240000\n",
      "action2      0.60    0.20   -0.40  0.000000 -0.400000\n",
      "action3      0.40    0.40   -0.60 -0.266667  0.066667\n",
      "romantic1   -0.65   -0.15    0.35  0.350000  0.100000\n",
      "romantic2   -0.08   -0.08    0.52 -0.480000  0.120000\n",
      "romantic3   -0.35   -0.60    0.40  0.150000  0.400000\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>action1</th>\n",
       "      <th>action2</th>\n",
       "      <th>action3</th>\n",
       "      <th>romantic1</th>\n",
       "      <th>romantic2</th>\n",
       "      <th>romantic3</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>action1</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.706689</td>\n",
       "      <td>0.813682</td>\n",
       "      <td>-0.799411</td>\n",
       "      <td>-0.025392</td>\n",
       "      <td>-0.914106</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>action2</th>\n",
       "      <td>0.706689</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.723102</td>\n",
       "      <td>-0.845154</td>\n",
       "      <td>-0.518999</td>\n",
       "      <td>-0.843374</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>action3</th>\n",
       "      <td>0.813682</td>\n",
       "      <td>0.723102</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>-0.847946</td>\n",
       "      <td>-0.379980</td>\n",
       "      <td>-0.802181</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>romantic1</th>\n",
       "      <td>-0.799411</td>\n",
       "      <td>-0.845154</td>\n",
       "      <td>-0.847946</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.148039</td>\n",
       "      <td>0.723747</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>romantic2</th>\n",
       "      <td>-0.025392</td>\n",
       "      <td>-0.518999</td>\n",
       "      <td>-0.379980</td>\n",
       "      <td>0.148039</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.393939</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>romantic3</th>\n",
       "      <td>-0.914106</td>\n",
       "      <td>-0.843374</td>\n",
       "      <td>-0.802181</td>\n",
       "      <td>0.723747</td>\n",
       "      <td>0.393939</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            action1   action2   action3  romantic1  romantic2  romantic3\n",
       "action1    1.000000  0.706689  0.813682  -0.799411  -0.025392  -0.914106\n",
       "action2    0.706689  1.000000  0.723102  -0.845154  -0.518999  -0.843374\n",
       "action3    0.813682  0.723102  1.000000  -0.847946  -0.379980  -0.802181\n",
       "romantic1 -0.799411 -0.845154 -0.847946   1.000000   0.148039   0.723747\n",
       "romantic2 -0.025392 -0.518999 -0.379980   0.148039   1.000000   0.393939\n",
       "romantic3 -0.914106 -0.843374 -0.802181   0.723747   0.393939   1.000000"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def standardize(row):\n",
    "    new_row = (row - row.mean())/(row.max()-row.min())\n",
    "    return new_row\n",
    "\n",
    "df_std = ratings.apply(standardize).T\n",
    "print(df_std)\n",
    "\n",
    "sparse_df = sparse.csr_matrix(df_std.values)\n",
    "corrMatrix = pd.DataFrame(cosine_similarity(sparse_df),index=ratings.columns,columns=ratings.columns)\n",
    "corrMatrix"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>action1</th>\n",
       "      <th>action2</th>\n",
       "      <th>action3</th>\n",
       "      <th>romantic1</th>\n",
       "      <th>romantic2</th>\n",
       "      <th>romantic3</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>action1</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.706689</td>\n",
       "      <td>0.813682</td>\n",
       "      <td>-0.799411</td>\n",
       "      <td>-0.025392</td>\n",
       "      <td>-0.914106</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>action2</th>\n",
       "      <td>0.706689</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.723102</td>\n",
       "      <td>-0.845154</td>\n",
       "      <td>-0.518999</td>\n",
       "      <td>-0.843374</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>action3</th>\n",
       "      <td>0.813682</td>\n",
       "      <td>0.723102</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>-0.847946</td>\n",
       "      <td>-0.379980</td>\n",
       "      <td>-0.802181</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>romantic1</th>\n",
       "      <td>-0.799411</td>\n",
       "      <td>-0.845154</td>\n",
       "      <td>-0.847946</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.148039</td>\n",
       "      <td>0.723747</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>romantic2</th>\n",
       "      <td>-0.025392</td>\n",
       "      <td>-0.518999</td>\n",
       "      <td>-0.379980</td>\n",
       "      <td>0.148039</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.393939</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>romantic3</th>\n",
       "      <td>-0.914106</td>\n",
       "      <td>-0.843374</td>\n",
       "      <td>-0.802181</td>\n",
       "      <td>0.723747</td>\n",
       "      <td>0.393939</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            action1   action2   action3  romantic1  romantic2  romantic3\n",
       "action1    1.000000  0.706689  0.813682  -0.799411  -0.025392  -0.914106\n",
       "action2    0.706689  1.000000  0.723102  -0.845154  -0.518999  -0.843374\n",
       "action3    0.813682  0.723102  1.000000  -0.847946  -0.379980  -0.802181\n",
       "romantic1 -0.799411 -0.845154 -0.847946   1.000000   0.148039   0.723747\n",
       "romantic2 -0.025392 -0.518999 -0.379980   0.148039   1.000000   0.393939\n",
       "romantic3 -0.914106 -0.843374 -0.802181   0.723747   0.393939   1.000000"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "corrMatrix = ratings.corr(method='pearson')\n",
    "corrMatrix.head(6)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [],
   "source": [
    "def get_similar(movie_name,rating):\n",
    "    similar_score = corrMatrix[movie_name]*(rating-2.5)\n",
    "    similar_score = similar_score.sort_values(ascending=False)\n",
    "    #print(type(similar_ratings))\n",
    "    return similar_score"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>action1</th>\n",
       "      <th>action2</th>\n",
       "      <th>action3</th>\n",
       "      <th>romantic1</th>\n",
       "      <th>romantic2</th>\n",
       "      <th>romantic3</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>2.500000</td>\n",
       "      <td>1.766722</td>\n",
       "      <td>2.034204</td>\n",
       "      <td>-1.998527</td>\n",
       "      <td>-0.063480</td>\n",
       "      <td>-2.285265</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>0.038088</td>\n",
       "      <td>0.778499</td>\n",
       "      <td>0.569970</td>\n",
       "      <td>-0.222059</td>\n",
       "      <td>-1.500000</td>\n",
       "      <td>-0.590909</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1.371159</td>\n",
       "      <td>1.265061</td>\n",
       "      <td>1.203271</td>\n",
       "      <td>-1.085620</td>\n",
       "      <td>-0.590909</td>\n",
       "      <td>-1.500000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    action1   action2   action3  romantic1  romantic2  romantic3\n",
       "0  2.500000  1.766722  2.034204  -1.998527  -0.063480  -2.285265\n",
       "1  0.038088  0.778499  0.569970  -0.222059  -1.500000  -0.590909\n",
       "2  1.371159  1.265061  1.203271  -1.085620  -0.590909  -1.500000"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "action_lover = [(\"action1\",5),(\"romantic2\",1),(\"romantic3\",1)]\n",
    "similar_scores = pd.DataFrame()\n",
    "for movie,rating in action_lover:\n",
    "    similar_scores = similar_scores.append(get_similar(movie,rating),ignore_index = True)\n",
    "\n",
    "similar_scores.head(10)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "action1      3.909247\n",
       "action2      3.810282\n",
       "action3      3.807445\n",
       "romantic2   -2.154389\n",
       "romantic1   -3.306206\n",
       "romantic3   -4.376174\n",
       "dtype: float64"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "similar_scores.sum().sort_values(ascending=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.1"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
