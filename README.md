#include <stdio.h>

#define NUM_MONTHS 12

int main() {
    FILE *file;
    double temperature;
    double monthly_totals[NUM_MONTHS] = {0};
    int counts[NUM_MONTHS] = {0};
    double monthly_averages[NUM_MONTHS] = {0};
    const char *month_names[NUM_MONTHS] = {"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"};
    double coldest_temp = 100.0; // Initialize to a high value
    double warmest_temp = -100.0; // Initialize to a low value
    int coldest_month = -1;
    int warmest_month = -1;
    int month;

    // Open the file
    file = fopen("temperature_data.txt", "r");
    if (file == NULL) {
        printf("Unable to open the file.\n");
        return 1;
    }

    // Read temperatures and calculate monthly totals and counts
    month = 0;
    while (fscanf(file, "%lf", &temperature) == 1) {
        monthly_totals[month] += temperature;
        counts[month]++;
        month = (month + 1) % NUM_MONTHS; // Increment month, but wrap around to 0 after December
    }

    // Calculate monthly averages and identify coldest and warmest months
    for (month = 0; month < NUM_MONTHS; month++) {
        if (counts[month] != 0) {
            monthly_averages[month] = monthly_totals[month] / counts[month];
            if (monthly_averages[month] < coldest_temp) {
                coldest_temp = monthly_averages[month];
                coldest_month = month;
            }
            if (monthly_averages[month] > warmest_temp) {
                warmest_temp = monthly_averages[month];
                warmest_month = month;
            }
        }
    }

    // Close the file
    fclose(file);

    // Print monthly averages and identify coldest and warmest months
    printf("Monthly Average Temperature from 1900 to 2015:\n");
    for (month = 0; month < NUM_MONTHS; month++) {
        printf("%s: %.2lf\n", month_names[month], monthly_averages[month]);
    }

    printf("\nColdest Month: %s (%.2lf degrees)\n", month_names[coldest_month], coldest_temp);
    printf("Warmest Month: %s (%.2lf degrees)\n", month_names[warmest_month], warmest_temp);

    return 0;
}
