import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
df = pd.read_csv('fcc-forum-pageviews.csv', index_col='date', parse_dates=True)

# Clean the data by removing the top and bottom 2.5% of the page views
lower_percentile = df['page_views'].quantile(0.025)
upper_percentile = df['page_views'].quantile(0.975)
df_clean = df[(df['page_views'] >= lower_percentile) & (df['page_views'] <= upper_percentile)]

# Function to draw the line plot
def draw_line_plot():
    plt.figure(figsize=(10, 6))
    plt.plot(df_clean.index, df_clean['page_views'], color='blue', linewidth=1)
    plt.title('Daily freeCodeCamp Forum Page Views 5/2016-12/2019')
    plt.xlabel('Date')
    plt.ylabel('Page Views')
    plt.tight_layout()
    plt.savefig('line_plot.png')
    plt.show()

# Function to draw the bar plot
def draw_bar_plot():
    df_clean['year'] = df_clean.index.year
    df_clean['month'] = df_clean.index.month
    monthly_avg = df_clean.groupby(['year', 'month']).agg({'page_views': 'mean'}).unstack()

    monthly_avg.plot(kind='bar', figsize=(10, 6), legend=True)
    plt.title('Average Daily Page Views per Month Grouped by Year')
    plt.xlabel('Years')
    plt.ylabel('Average Page Views')
    plt.legend(title='Months', labels=[f'{i+1}' for i in range(12)])
    plt.tight_layout()
    plt.savefig('bar_plot.png')
    plt.show()

# Function to draw the box plot
def draw_box_plot():
    df_clean['year'] = df_clean.index.year
    df_clean['month'] = df_clean.index.month

    plt.figure(figsize=(14, 6))

    # Create the first box plot (Year-wise)
    plt.subplot(1, 2, 1)
    sns.boxplot(x='year', y='page_views', data=df_clean)
    plt.title('Year-wise Box Plot (Trend)')
    plt.xlabel('Year')
    plt.ylabel('Page Views')

    # Create the second box plot (Month-wise)
    plt.subplot(1, 2, 2)
    sns.boxplot(x='month', y='page_views', data=df_clean)
    plt.title('Month-wise Box Plot (Seasonality)')
    plt.xlabel('Month')
    plt.ylabel('Page Views')
    plt.xticks(ticks=range(12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])

    plt.tight_layout()
    plt.savefig('box_plot.png')
    plt.show()

# Call the functions to generate the plots
draw_line_plot()
draw_bar_plot()
draw_box_plot()
