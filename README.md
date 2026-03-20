# BCOG200_Final_Project [SVM on ERP data (data source: open-source cleaned N400 data)]
# paper of reference: DOI: 10.1111/psyp.14570


# The planned project is to create a pipeline for running SVM on ERP data; The project comes from a research question of 'can machine learning methods define a more precise time window for an Event-Related Potential component Nref, whose temporal boundaries are not clearly defined'. current literature studied Nref within a very broad window of 400-1200ms. The goal of this project is to replicate the N400 from Luck's paper on python, and the same model will be used with my first year project of NRef signal to cross-validate the Nref effect



class Participant:
    def __init__(self, subj_id):
        self.subj_id=subj_id
        
        
        self.cond_1_trials=None
        self.cond_2_trials=None
        self.epoch_time=None
		
    	self.sub_accuracy=None
		self.time_point_accuracy=[]
		self.max_accuracy_window=None


	def load_data():
		pass
		#x,y=read data


def equate_trials(x,y):
	pass
	#min(x,y) this is for getting the equal num of trials for svm(go with the smaller trials across conditions)
	#Return x, y


def pseudo_trials(data, num_trials):
  #this creates pseudotrials for train and test
	Return matrix

def combine_matrix(matrix1, matrix2):
  #combine the matrix of two conditions
  return full_matrix, y_col


def run_svm(full_matrix, y_col):
	#svm run on every time point and get accuracy at each time point
	return snapshot_accuracy

def time_window_compare(interval):
	#Cut the time_point_accuracy into n interval, get the average accuracy across diff time window e.g. 100ms, 200ms...
	#Compare accuracy across time window
	return time_window_of_highest_accuracy


def average_accuracy(time_point_accuracy):
	pass
	#Average across all time points



def group_accuracy(sub_accuracy):
	pass
	#Average across group








import numpy as np
import mne

class Participant:

    def __init__(self, subject, base="./STUDY5subjects"):
        self.subject = subject
        self.syn_path = f"{base}/{subject}/syn.set"
        self.non_syn_path = f"{base}/{subject}/non-syn.set"
        self.matrix_syn = None
        self.matrix_nonsyn = None
        self.syn_max_trials = None
        self.non_syn_max_trials = None

        self.sub_accuracy = None
        self.time_point_accuracy = []
        self.max_accuracy_window = None
        self.N400_window = None

    def load_data(self):
        self.epochs_syn = mne.read_epochs_eeglab(self.syn_path)
        # print(self.epochs_syn.get_data().shape)
        self.epochs_nonsyn = mne.read_epochs_eeglab(self.non_syn_path)
        self.epochs_syn.apply_baseline(baseline=(-0.2, 0.0))
        self.epochs_nonsyn.apply_baseline(baseline=(-0.2, 0.0))
        self.syn_max_trials = self.epochs_syn.get_data().shape[0]
        self.non_syn_max_trials = self.epochs_nonsyn.get_data().shape[0]
        self.epochtime = self.epochs_syn.get_data().shape[2]

    def equal_matrix_svm(self):
        min_trials = min(self.syn_max_trials, self.non_syn_max_trials)

        self.trials_for_pesudo = min_trials // 5

        self.matrix_syn = np.random.permutation(self.epochs_syn.get_data())[:min_trials]

        self.matrix_nonsyn = np.random.permutation(self.epochs_nonsyn.get_data())[
            :min_trials
        ]

    def create_pseudo_trials(self):
        pseudo_trial_list = []

        random_syn_matrix = np.random.permutation(self.matrix_syn)
        print(random_syn_matrix.shape)
        random_nonsyn_matrix = np.random.permutation(self.matrix_nonsyn)
        start_trial = 0
        end_trial = 5
        for time in range(self.trials_for_pesudo):
            syn_group = random_syn_matrix[start_trial:end_trial]
            nonsyn_group = random_nonsyn_matrix[start_trial:end_trial]
            average_syn = syn_group.mean(axis=0)
            average_nonsyn = nonsyn_group.mean(axis=0)
            pseudo_trial_list.append(average_syn)
            pseudo_trial_list.append(average_nonsyn)
            start_trial += 5
            end_trial += 5

        return np.array(pseudo_trial_list)


def main():
    participant_list = ["s02"]
    # , "s05", "s07", "s08", "s10"]
    for n in participant_list:
        n = Participant(n)
        # n.load_data()
        # n.equal_matrix_svm()
        # peudo = n.create_pseudo_trials()


if __name__ == "__main__":
    main()


	
